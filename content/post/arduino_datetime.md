+++
title='Arduino datetime 常用方法'
tags=[]
categories=["arduino"]
date="2022-12-04T22:32:02+08:00"
toc=true
draft=false
hiddenFromHomePage= false
+++


https://github.com/tobozo/ESP32-BLECollector/blob/master/ESP32-BLECollector/DateTime.h 
https://github.com/tobozo/ESP32-BLECollector/blob/master/ESP32-BLECollector/BLEFileSharing.h
https://github.com/PaulStoffregen/Time



```
DateTime UTCTime   = DateTime(BLERemoteTime.year, BLERemoteTime.month, BLERemoteTime.day, BLERemoteTime.hour, BLERemoteTime.minutes, BLERemoteTime.seconds);
  DateTime LocalTime = UTCTime.unixtime() + BLERemoteTime.tz * 3600;

  dumpTime("UTC:", UTCTime);
  dumpTime("Local:", LocalTime);

  setTime( LocalTime.unixtime() );

  timeval epoch = {(time_t)LocalTime.unixtime(), 0};
  const timeval *tv = &epoch;
  settimeofday(tv, NULL);

  struct tm now;
  getLocalTime(&now,0);

  Serial.printf("[Heap: %06d] Time has been set to: %04d-%02d-%02d %02d:%02d:%02d\n",
   freeheap,
   LocalTime.year(),
   LocalTime.month(),
   LocalTime.day(),
   LocalTime.hour(),
   LocalTime.minute(),
   LocalTime.second()
  );

```