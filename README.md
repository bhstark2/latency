# latency-explained
repo for development of the "latency explained" report

# Description

When people think of Internet performance it is typically solely in terms of speed. But when considering the end-user quality of experience (QoE), latency is usually a key factor. Unfortunately, latency is not well understood in the policy or regulatory spheres or by the average consumer. This paper will explain and explore latency, including idle latency, latency under load, components of latency performance, how latency varies by access technology and/or protocol, and how latency can affect the user QoE for certain types of applications such as video conferencing, remote learning, gaming and more. As well, while the FCC MBA program reports on latency it takes no position on what is good or bad latency — a question this paper will explore. Additionally, this paper will examine metrics for characterizing latency performance, including various summary statistics for latency and latency variation (often referred to as "jitter"). Finally, the paper will look to the current state of the art and the future to explain Active Queue Management (AQM) and other low latency services — and the new types of applications that this may enable in the future.

# Notes

Doc can be built with

```
pandoc --defaults=components/defaults.yaml --citeproc --bibliography=references.yaml --resource-path=.:components --output=output/report.pdf report.md
```
