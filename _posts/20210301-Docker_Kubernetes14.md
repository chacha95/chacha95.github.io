---
layout: post
title: 딥러닝을 위한 kubernetes 7 - kubectl
tags: [MLOps]
use_math: true
---

# Kubectl 소개

쿠버네티스의 전체 파이프라인은 다음과 같습니다.

> 전체 파이프라인

![](https://user-images.githubusercontent.com/31475037/94405746-f700eb00-01ab-11eb-8ec5-73b27a336aa0.png)



Kubectl 명령어를 실행하면 kube-apiserver에 REST API로 yaml 혹은 json으로 작성된 파일이 전송됩니다. kubectl이 kube-apiserver에 yaml 파일 전송시, JSON 형식으로 정보를 변환시켜 전송합니다.

이를 통해 kube-apiserver를 관리하는 구조이며 이를 잘 알아야 전체 노드와 파드 관리가 가능합니다.



<br>

**참조 강의**

[kubectl](https://kubernetes.io/ko/docs/reference/kubectl/overview/)

[kubectl cheat sheet](https://kubernetes.io/ko/docs/reference/kubectl/cheatsheet/)

[docker 사용자를 위한 kubectl](https://kubernetes.io/ko/docs/reference/kubectl/docker-cli-to-kubectl/)