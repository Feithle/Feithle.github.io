---
layout: post
date: 2019-09-03 14:00:00
title: "hadoop 计算 pi"
excerpt: "随机点法计算圆周率，生成的随机点越多得到的pi值越精确"
---

### 用Hadoop计算pi值

{% raw %}
Hadoop自带的例子中，有一个计算Pi值的例子。这个程序的原理是这样的。假如有一个边长为1的正方形。以正方形的一个端点为圆心，以1为半径，画一个圆弧，于是在正方形内就有了一个直角扇形。在正方形里随机生成若干的点，则有些点是在扇形内，有些点是在扇形外。正方形的面积是1，扇形的面积是0.25*Pi。设点的数量一共是n，扇形内的点数量是nc，在点足够多足够密集的情况下，会近似有nc/n的比值约等于扇形面积与正方形面积的比值，也就是nc/n= 0.25*Pi/1，即Pi = 4*nc/n。
如何生成随机点？最简单的方式是在[0,1]的区间内每次生成两个随机小数作为随机点的x和y坐标。可惜这种生成方式效果不够好，随机点之间有间隙过大和重叠的可能，会让计算精度不够高。Halton序列算法生成样本点的效果要好得多，更均匀，计算精度比随机生成的点更高，因此这个例子用Halton序列算法生成点集
{% endraw %}

```java
package org.apache.hadoop.pi;

import java.io.IOException;
import java.util.Random;
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.DoubleWritable;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
public class Pi {
	
	/**
	 * 
	 * @author liqiyuan
	 * date:2019-9-3
	 * todo:calculate the pi's value
	 *
	 */
	public static class PiMapper extends Mapper<Object, Text, Text, IntWritable>{
		private static Random rd = new Random();
		public void map(Object key, Text value, Context context) throws IOException, InterruptedException {
			// TODO Auto-generated method stub
			int pointNum = Integer.parseInt(value.toString());
			
			for (int i = 0; i < pointNum; i++) {
				//get random number
				double x = rd.nextDouble();
				double y = rd.nextDouble();
				//calculate distance 
				x-=0.5;
				y-=0.5;
				double distance = Math.sqrt(x*x + y*y);
				IntWritable result = new IntWritable(0);
				
				if (distance <= 0.5) {
					result = new IntWritable(1);
				}
				context.write(value, result);
			}

		}
	}

	public static class PiReducer extends Reducer<Text, IntWritable, Text, DoubleWritable>{
		private DoubleWritable resule = new DoubleWritable();
		
		public void reduce(Text key,Iterable<IntWritable> values,Context context) throws IOException, InterruptedException {
			double pointNum  = Double.parseDouble(key.toString());
			double sum = 0;
			for (IntWritable val : values) {
				sum+=val.get();
			}
			resule.set(sum/pointNum*4);
			context.write(key, resule);
		}
	}
	
	public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
		Configuration conf = new Configuration();
		conf.set("fs.defaultFS", "hdfs://127.0.0.1:9000");
		System.setProperty("HADOOP_USER_NAME", "hadoop");
		Job job = Job.getInstance(conf, "calcukate");
		job.setJarByClass(Pi.class);
		job.setMapperClass(PiMapper.class);
		//job.setCombinerClass(PiReducer.class);
		job.setReducerClass(PiReducer.class);
		job.setMapOutputKeyClass(Text.class);
		job.setMapOutputValueClass(IntWritable.class);
		job.setOutputKeyClass(Text.class);
		job.setOutputValueClass(DoubleWritable.class);
		Path inputPath = new Path("hdfs://localhost:9000/user/hadoop/day04/");
		Path outputPath = new Path("hdfs://localhost:9000/user/hadoop/day04/output");
		
		FileInputFormat.addInputPath(job, inputPath);
	
		FileOutputFormat.setOutputPath(job, outputPath);
		System.exit(job.waitForCompletion(true)? 0:1);		
	}
}

```
