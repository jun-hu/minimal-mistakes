---
layout: posts
title: Daemonsets
date: 2019-02-26 16:00:00 +0900
type: posts
published: true
comments: true
categories: [dockerkubernetes]
tags: [kubernetes]
---


DaemonSet
A DaemonSet ensures that all (or some) Nodes run a copy of a Pod. As nodes are added to the cluster, Pods are added to them. As nodes are removed from the cluster, those Pods are garbage collected. Deleting a DaemonSet will clean up the Pods it created.
Some typical uses of a DaemonSet are:
• running a cluster storage daemon, such as glusterd, ceph, on each node.
• running a logs collection daemon on every node, such as fluentd or logstash.
• running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Dynatrace OneAgent, AppDynamics Agent, Datadog agent, New Relic agent, Ganglia gmond or Instana agent.
Some typical uses of a DaemonSet are:
• running a cluster storage daemon, such as glusterd, ceph, on each node.
• running a logs collection daemon on every node, such as fluentd or logstash.
• running a node monitoring daemon on every node, such as Prometheus Node Exporter, collectd, Dynatrace OneAgent, AppDynamics Agent, Datadog agent, New Relic agent, Ganglia gmond or Instana agent.

하나의 데몬셋에서 여러 플레그 값을 부여함으로써 다양하게 사용할 수 있다.
Writing a DaemonSet Spec
Create a DaemonSet
You can describe a DaemonSet in a YAML file. For example, the daemonset.yaml file below describes a DaemonSet that runs the fluentd-elasticsearch Docker image:
controllers/daemonset.yaml 
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: k8s.gcr.io/fluentd-elasticsearch:1.20
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
• Create a DaemonSet based on the YAML file: kubectl create -f https://k8s.io/examples/controllers/daemonset.yaml

