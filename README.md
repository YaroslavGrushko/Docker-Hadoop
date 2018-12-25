# Docker Hadoop
# This project was created with https://github.com/zoltan-nz/docker-hadoop-ubuntu 

Hadoop docker:  
http://msoliman.me/2017/04/24/hadoop-inside-docker-the-easiest-way-in-5-minutes/   
Hadoop for windows:  
https://hadoop.apache.org/  
dockerfile build Hadoop with java  
https://github.com/zoltan-nz/docker-hadoop-ubuntu  

Hadoop home: /usr/local/hadoop (HADOOP_PREFIX in github repo above)
This is work directory: /home/work

Tutorial of Hadoop commands:
https://www.tutorialspoint.com/hadoop/hadoop_quick_guide.htm 
# Prerequests  
Docker installed  
# Get  Started  

1. We picked up Ubuntu on the docker, Hadoop + java on it (at the same time, the work files {ProcessUnits.java, hadoop-core-1.2.1.jar, sample.txt} were thrown into home/work)  
  a. docker-compose up  
  b. Connect to the container via **docker-compose exec home sh**  
2. I went to the home/work directory and created the units folder there  
3. Compiled ProcessUnits.java, while specifying the library file via -classpath hadooop-core-1.2.1.jar, and the target folder as -d units  
  a. **$ javac -classpath hadoop-core-1.2.1.jar -d units ProcessUnits.java** 
  b. Created a .jar file in the units folder  
  **$ jar -cvf units.jar -C units/.**  
4. Created a directory of input files in Hadup:  
**$ HADOOP_PREFIX/bin/hadoop fs -mkdir input_dir**  
5. Copy the input file sample.txt from the home/work directory to the folder you just created in Hadup:  
**$ HADOOP_PREFIX/bin/hadoop fs -put /home/work/sample.txt input_dir**  
{(optional, you can delete the file):  
**$ HADOOP_PREFIX/bin/hadoop fs -rm input_dir/sample.txt   
$ HADOOP_PREFIX/bin/hadoop dfs -rmr output_dir**}  
6. I will check everything that lies in input_dir in the hadup:  
   **$ HADOOP_PREFIX/bin/hadoop fs -ls input_dir/**  
7. Now I run (compiled earlier .jar file) using the hadup, while at the same time I submit input_dir / sample.txt to the input:
NOTE: Checking if I’m in the home/work directory should be there.  
      **$ HADOOP_PREFIX/bin/hadoop jar units.jar hadoop.ProcessUnits input_dir output_dir**  
8. I look through the output files in the output folder of the Hadoop:  
**$ HADOOP_PREFIX/bin/hadoop fs -ls output_dir/**
9. Browse the part-00000 file, which is located in the output folder of the hadup:  
             **$ HADOOP_PREFIX/bin/hadoop fs -cat output_dir/part-00000**
10. Copy the output folder from the Hadup to the host (in the home/work working folder)
      **$ HADOOP_PREFIX/bin/hadoop dfs -get output_dir/ /home/work/**
