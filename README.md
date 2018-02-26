# FiletoKafka
I was trying to use kafka console producer to consume a csv file with around 4000000 records and was able to consume the data only partially, so this is the bash script I wrote to partition the csv into small chunks and then "cat" into kafka console-producer


!/bin/bash

#Preparation of file to import into kafka topic
read -p 'Enter the file name: ' filename
dir="${filename%%.*}"
mkdir $dir
cp $filename $dir/
cd $dir
split -l 100000 $filename splits
rm $filename
read -p 'Enter Kafka topic name: ' topicname


#Cat into kafka consumer
for file in /usr/lib/kafka/$dir/*; do

echo ${file##*/}
cat ${file##*/} | kafka-console-producer --broker-list localhost:9092 --topic $$
#wait
done
