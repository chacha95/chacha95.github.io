---
layout: post
title: Full Stack Deep Learning 3 - data management
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

Where training data comes from

Most DL applications require lots of labeled data Exceptions: RL, GANs, "semi-supervised" learning Publicly available datasets = No competitive advantage But can serve as starting point

> 레이블링엔 돈이 많이 든다....

![](https://user-images.githubusercontent.com/31475037/92839281-bae33180-f41a-11ea-8e5c-cbeaa10e1417.png)



## Labeling with UI/UX

How to label proprietary data at scale

> 

![](https://user-images.githubusercontent.com/31475037/92839276-b9b20480-f41a-11ea-8a4c-b8ad30ed49bc.png)





> 스스로 학습하는 딥러닝

![](https://user-images.githubusercontent.com/31475037/92839274-b880d780-f41a-11ea-9ed0-17476e0b633a.png)





> 

![](https://user-images.githubusercontent.com/31475037/92839271-b7e84100-f41a-11ea-91f3-53534516bcb0.png)





> 

![](https://user-images.githubusercontent.com/31475037/92839268-b74faa80-f41a-11ea-9b9c-54dc39334ec8.png)

### Sources of labor

Hire own annotators, promote best ones to quality control • Pros: secure, fast (once hired), less QC needed • Cons: expensive, slow to scale, admin overhead • ...or, crowdsource (Mechanical Turk) • Pros: cheaper, more scalable • Cons: not secure, significant QC effort required • ...or, full-service data labeling companies

### Service companies

Data labeling requires separate software stack, temporary labor, and quality assurance. Makes sense to outsource. • Dedicate several days to selecting the best one for you: • Label gold standard data yourself • Sales calls with several contenders, ask for work sample on same data • Ensure agreement with your gold standard, and evaluate on value

Full-service data labeling is always pricy • But some companies offer their software without labor

Conclusions • outsource to full-service company if you can afford it • if not, then at least use existing software • hiring part-time makes more sense than trying to make crowdsourcing work

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