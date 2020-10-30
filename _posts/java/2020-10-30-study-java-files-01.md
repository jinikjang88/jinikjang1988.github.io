---
layout: post
title: "Java 특정 시간 지나간 파일 삭제 하기"
subheading: Java 파일 삭제
author: Jeffrey
categories: Java
banner: "/assets/images/banners/java.jpeg"
tags: Java
---

## 24시간 지난 파일 삭제 하기

파일 업로드 후 임시폴더에 있는 파일들 중 24시간이 지난 파일을 삭제 하기 위해 스케줄을 등록하여 삭제 하는 로직을 만들었다.

## 개발절차
    1. 스케줄 컴포넌트 만들기
    2. 로직 작성하기
    3. 검증 하기

### 1. 스케줄 컴포넌트 만들기

~/src/main/java/com/ikjini/filemanager/support/component/ExpiredFilesScheduler.java


### 2. 로직 작성하기
```java
package com.ikjini.filemanager.support.component;

import com.ikjini.filemanager.config.file.property.FileStorageProperties;
import java.io.File;
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.time.Instant;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.stream.Collectors;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

@Component
public class ExpiredFilesScheduler {

  private static final Logger logger = LoggerFactory.getLogger(Scheduled.class);

  private final Path uploadLocation;

  public ExpiredFilesScheduler(FileStorageProperties fileStorageProperties) {
    this.uploadLocation = Paths.get(fileStorageProperties.getUploadDir())
        .toAbsolutePath()
        .normalize();

  }

  @Scheduled(cron = "0 * * * * *")
  public void deleteExpiredFilesDaily() {
    String SchedulerStartTime = LocalDateTime.now()
        .format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));

    logger.info("Expired File delete Scheduler : " + SchedulerStartTime);

    // 삭제 대상 파일 가져오기
    List<Path> expiredFiles = this.getExpiredFiles();

    // 삭제 대상 파일을 하나씩 삭제
    for (Path path : expiredFiles) {
      try {
        logger.info("FILE DELETED : " + path.toAbsolutePath());
        Files.delete(path);
      } catch (IOException e) {
        e.printStackTrace();
      }
    }
  }

  // 삭제 대상 파일 리턴 메소드
  public List<Path> getExpiredFiles() {

    LocalDateTime expiredModifiedTime = LocalDateTime.now().minusHours(24);
    try {
      // 24시간 지난 임시저장 파일 추출
      // this.uploadLocation 은 로컬의 파일디렉토리 Path
      List<Path> deleteTargetFile = Files.list(this.uploadLocation).filter(
          path -> {
            File file = new File(String.valueOf(path));

            // 현재 파일의 마지막 수정 시간
            LocalDateTime modifiedTime = Instant.ofEpochMilli(file.lastModified()).atZone(
                ZoneId.systemDefault()).toLocalDateTime();

            // 24시간 파일 추출 return path
//            logger.info("DELETE TARGET : " + file.getAbsolutePath() + ", " + modifiedTime);
            return modifiedTime.isBefore(expiredModifiedTime);
          }
      ).collect(Collectors.toList());

      return deleteTargetFile;
    } catch (IOException e) {
      e.printStackTrace();
    }
    return null;
  }
}


```

### 3. 검증하기

```log
2020-10-30 17:08:00.004  INFO 41383 --- [   scheduling-1] o.s.scheduling.annotation.Scheduled      : Expired File delete Scheduler : 2020-10-30 17:08:00
2020-10-30 17:10:00.001  INFO 41383 --- [   scheduling-1] o.s.scheduling.annotation.Scheduled      : Expired File delete Scheduler : 2020-10-30 17:10:00
2020-10-30 17:10:00.005  INFO 41383 --- [   scheduling-1] o.s.scheduling.annotation.Scheduled      : FILE DELETED : /Users/mz02-jijang/IdeaProjects/filemanager/src/main/resources/files/temp/bf3c6335-549a-4611-b8da-091ddb18ffc3.png
2020-10-30 17:10:00.005  INFO 41383 --- [   scheduling-1] o.s.scheduling.annotation.Scheduled      : FILE DELETED : /Users/mz02-jijang/IdeaProjects/filemanager/src/main/resources/files/temp/33414de0-0be3-4fdc-8fba-a9498318f227.png
2020-10-30 17:10:00.005  INFO 41383 --- [   scheduling-1] o.s.scheduling.annotation.Scheduled      : FILE DELETED : /Users/mz02-jijang/IdeaProjects/filemanager/src/main/resources/files/temp/8a850df9-c32d-4f8b-8397-883b2fd4c1e2.png
```
