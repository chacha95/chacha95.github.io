---
layout: post
title: Full Stack Deep Learning 2 - data management
tags: [Full Stack Deep Learning]
use_math: true
---

# Data Management



> 

![](https://user-images.githubusercontent.com/31475037/92839296-be76b880-f41a-11ea-815e-3fe6d5fdae59.png)

Not enough data • Class imbalances • Noisy labels • Train / test from different distributions • etc

> 

![](https://user-images.githubusercontent.com/31475037/92839290-bd458b80-f41a-11ea-876d-0d7c1838c68c.png)



## Sources

대부분의 DL 애플리케이션에는 많은 레이블이 지정된 데이터가 필요합니다. (예외: RL, GAN, semi-supervised learning) 

> 레이블링엔 돈이 많이 든다....

![](https://user-images.githubusercontent.com/31475037/92839281-bae33180-f41a-11ea-8e5c-cbeaa10e1417.png)



## Labeling with UI/UX



> 

![](https://user-images.githubusercontent.com/31475037/92839276-b9b20480-f41a-11ea-8a4c-b8ad30ed49bc.png)





> 스스로 학습하는 딥러닝

![](https://user-images.githubusercontent.com/31475037/92839274-b880d780-f41a-11ea-9ed0-17476e0b633a.png)





> 

![](https://user-images.githubusercontent.com/31475037/92839271-b7e84100-f41a-11ea-91f3-53534516bcb0.png)





> 

![](https://user-images.githubusercontent.com/31475037/92839268-b74faa80-f41a-11ea-9b9c-54dc39334ec8.png)

### Sources of labor

자체 애노 테이터를 고용하고 최고의 애노 테이터를 품질 관리로 승격
• 장점 : 안전하고 빠르며 (고용 후) QC 필요 감소
• 단점 : 비용이 많이 들고 확장 속도가 느리고 관리 오버 헤드

crowdsource (Mechanical Turk)
• 장점 : 더 저렴하고 확장 가능
• 단점 : 안전하지 않음, 상당한 QC 노력 필요

### Service companies

데이터 라벨링에는 별도의 소프트웨어 스택, 임시 인력 및 품질 보증이 필요합니다. 아웃소싱하는 것이 합리적입니다. 

• 자신에게 가장 적합한 데이터를 선택하는 데 며칠을 바칩니다. 

• 중요한 데이터에 직접 라벨을 지정합니다. 

• 여러 경쟁자와 판매 전화를 걸어 동일한 데이터에 대한 작업 샘플을 요청합니다. 

풀 서비스 데이터 라벨링은 항상 비쌉니다.

결론

• 감당할 수있는 경우 풀 서비스 회사에 아웃소싱 

• 그렇지 않은 경우 최소한 기존 소프트웨어 사용 

• 크라우드 소싱 작업을 수행하는 것보다 파트 타임 고용이 더 합리적입니다.

## Data Storage

Data (images, sound files, etc) and metadata (labels, user activity) should be stored appropriately

### Building blocks

**Filesystem**

Foundational layer of storage. • Fundamental unit is a "file", which can be text or binary, is not versioned, and is easily overwritten. • Can be as simple as a locally mounted disk containing all the files you need. • Can be networked (e.g. NFS): accessible over network by multiple machines. • Can be distributed (e.g. HDFS): stored and accessed over multiple machines. • Be mindful of access patterns! Can be fast, but not parallel.

**Object Storage**

An API over the filesystem. GET, PUT, DELETE files to a service, without worrying where they are actually stored. • Fundamental unit is an "object". Usually binary: image, sound file, etc. • Versioning, redundancy can be built into the service. • Can be parallel, but not fast. • Amazon S3 is the canonical example. • For on-prem setup, Ceph is a good solution.

**Database**

Persistent, fast, scalable storage and retrieval of structured data. • Mental model: everything is actually in RAM, but software ensures that everything is logged to disk and never lost. • Fundamental unit is a "row": unique id, references to other rows, values in columns. • Not for binary data! Store references instead. • Usually for data that will be accessed again (not logs). • Postgres is the right choice >90% of the time. Best-in-class SQL, great support for unstructured JSON, actively developed.

SQL is the right interface for structured data. • If you avoid using it, you will eventually reinvent a slow and crappy subset of it.







**Data Lake**

Unstructured aggregation of data from multiple sources, e.g. databases, logs, expensive data transformations. • "Schema on read": dump everything in, then transform for specific needs later.

![](https://user-images.githubusercontent.com/31475037/92839266-b61e7d80-f41a-11ea-9c82-47628181d077.png)



![](https://user-images.githubusercontent.com/31475037/92839263-b585e700-f41a-11ea-9ff0-2e20ee5e43b9.png)



![](https://user-images.githubusercontent.com/31475037/92839260-b4ed5080-f41a-11ea-80d0-d6ad5b444c8c.png)



![](https://user-images.githubusercontent.com/31475037/92839257-b454ba00-f41a-11ea-929f-1b792afba3f4.png)





![](https://user-images.githubusercontent.com/31475037/92839240-b0c13300-f41a-11ea-9b01-3b4a2fa922f3.png)

<br>

**강의**

[Full Stack Deep Learning](https://course.fullstackdeeplearning.com/)