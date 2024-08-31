---
layout: post
title: 7 Key Engineering Metrics for Web Services
date: '2018-11-19 08:00:00'
tags:
- from-medium
splash: /assets/imgs/7-key-engineering-metrics-for-web-services/mitchel-boot-hOf9BaYUN88-unsplash.jpg
---

You are ready to launch a brand new service — it can be a stand-alone user facing service, or a web of micro-services, or anything in-between. Whatever it is, you need to check its pulse every now and then in order to detect and respond to problems rapidly. This blog post identifies 7 key metrics that can help you do that. These are based on the [RED Metrics](https://www.weave.works/blog/the-red-method-key-metrics-for-microservices-architecture/) and [USE Metrics](http://www.brendangregg.com/usemethod.html) framework.

* * *

First lets talk about application level metrics.

## Queries Per Second (QPS)

It is what it says on the tin. You would typically average this over some interval of time ranging from 5 seconds to 5 hours depending on your traffic. In isolation this number will not mean a lot, but once you have established a baseline for your service, typically a few weeks after it is operational, this metric can detect anomalies in traffic pattern that can be used as a trigger for further investigation.

## Response Time

This is the time your service takes to compute a response, it does not take into account network latencies, which means that it does not necessarily measure what your user is experiencing, but it is a close approximation. Let’s say over the last 5 minutes your service served a 100 requests, the fastest response time was 200 ms and the slowest response time was 1200 ms, with most requests being in the 400 ms range. One obvious metric to measure is the average response over these 100 requests. However, that does not help us uncover the problems faced by the request that took 1200 ms. To get a pessimistic estimate of the response time we could measure the 95th percentile (a.k.a p95) response time. Now we start seeing numbers closer to 1200 ms. But then we would chase after red herrings. To balance this out we measure two metrics here —

- _p50 response time_
- _p95 response time_

## Errors

For an HTTP based service the most obvious errors to monitor are the number of requests that are being returned with a 5xx HTTP status code. This indicates an Internal Server Error. Another HTTP status code worth monitoring is 4xx. Even though this indicates client error, seeing a higher than usual amount should sound an alarm. While we can measure these the way we measured QPS, by establishing a baseline and then looking for anomalies, there is a better way to track errors that can be useful in setting up automated alerts. That is to measure these errors as a proportion of successful responses as indicated by 2xx HTTP status codes. In addition to these two metrics, a third one we can track is the number of ERROR and WARN level logs emitted by our application. This is so that if we are doing some sort of graceful error recovery in our code, we might not see the dreaded 5xx, but we will see an increase in the number of ERROR or WARN logs indicating a problem.

- _Proportion of 5xx over 2xx HTTP responses_
- _Proportion of 4xx over 2xx HTTP responses_
- _Number of ERROR/WARN logs_

* * *

Now, let’s talk about system level metrics, where we measure how well we are using the computing resources in our server cluster — whether they are running in a public cloud like AWS, or in our own datacenter.

## CPU Utilization

Again, pretty much what it says on the tin. Most cloud providers will provide this out of the box for compute resources like VMs or Containers. For most applications, average CPU utilization is at around the 20% mark (higher for compute heavy applications), but it is often spiky at certain times where the utilization spikes up to say 80%. A good rule of thumb is to have your average utilization be less than half your max. A utilization of 100% is bad and requires immediate attention. You would typically set up alerts if it reaches above 85%. If you are monitoring all your micro-services, or all the instances of your web service, you would typically measure the median CPU utilization across all cluster.

## Memory Utilization

This too is an obvious one to track. Unlike CPU utilization, average memory utilization can be quite a bit higher than 20%, and often times it is **not** spiky. If there is a problem like a memory leak, it can grow gradually. For a cluster of servers it makes sense to track the median memory utilization.

## Network Traffic

While QPS is also a measure of network traffic, here we are measuring the number of bytes for both ingress and egress. Unlike other system level metrics, this is not a proportion because there is no good baseline to compare against. But severe anomalies like 0 incoming bytes can indicate a network partition in your service that you might have to investigate further.

## Storage Utilization

This measures the amount of disk storage capacity that is being utilized. Most cloud based services do not worry about storage capacity because they are using cloud storage systems like S3 or Dynamo DB. In these cases you might want to measure the proportion of new data added, because here you are paying for the total storage cost as well as data I/O.

* * *

That is it, the 7 metrics that can lead to healthy and happy web services!

> Photo by [Mitchel Boot](https://unsplash.com/@valeon?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/metrics?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

