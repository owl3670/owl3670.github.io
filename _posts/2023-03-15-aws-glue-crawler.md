---
layout: single
title: "[AWS] AWS Glue Crawler"

categories:
- AWS

toc: true
toc_sticky: true
toc_label: "Index"
toc_icon: "list"
---

# AWS Glue Crawler

---

## AWS Glue Crawler 란?

AWS Glue Crawler 는 AWS Glue 에서 사용할 데이터를 자동으로 탐색하고, 데이터의 스키마를 추출하는 기능을 제공하는 서비스이다.

## AWS Glue Crawler 사용법

### AWS Glue Crawler 생성

AWS Glue Crawler 를 생성하기 위해 AWS Glue Console 에서 Data Catalog > Crawlers 로 들어간다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_1.png)

Create Crawler 버튼을 클릭한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_2.png)

Crawler name 을 입력한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_3.png)

Crawler 가 탐색할 Datasource 를 추가 한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_4.png)

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_5.png)

폴더 탐색 옵션 설정 : Subsequent crawler runs

- Crawl all sub-folders : 하위 폴더를 모두 탐색한다.
- Crawl new sub-folders only : 새로 생성된 하위 폴더만 탐색한다.
- Crawl based on events : 이벤트를 기반으로 탐색한다.

iam role 을 설정한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_6.png)

datacatalog 를 생성할 database 와 scheduling 을 설정한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_7.png)

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_8.png)

scheduling 옵션

- On demand : 수동으로 실행한다.
- Hourly
- Daily
- Weekly
- Monthly
- Custom : cron 표현식으로 실행 주기를 설정한다.

입력한 내용을 확인 후 Crawler 를 생성한다.

![image](/assets/images/aws_glue_crawler/aws_glue_crawler_9.png)