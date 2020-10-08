# Install fluentd on windows

> https://docs.fluentd.org/installation/install-by-msi#td-agent-v4

### Step 1: Install td-agent

```
Download .msi
```

### Step 2: Run td-agent from Command Prompt

```
td-agent.conf 파일 생성
<source>
  @type forward
  bind "127.0.0.1"
  port "24224"
</source>
<match test.**>
  @type stdout
</match>

프로세스 실행(Td-agent Command Prompt)
> fluentd -c td-agent.conf

데이터 전송
> echo {"message":"hello"} | fluent-cat test.event
```
