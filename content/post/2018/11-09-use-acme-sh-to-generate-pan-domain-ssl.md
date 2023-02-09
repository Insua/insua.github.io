---
title: use acme.sh to generate pan-domain ssl
date: 2018-11-09 11:06:12
tags: 
 - ssl
---
download acme.sh
```
curl  https://get.acme.sh | sh
```
goto acme.sh directory
```
cd ~/.acme.sh
```
add issued domain
```
./acme.sh --issue --dns -d "*.domain" --yes-I-know-dns-manual-mode-enough-go-ahead-please
```
add txt dns record  
then renew
```
./acme.sh --issue --dns -d "*.domain" --yes-I-know-dns-manual-mode-enough-go-ahead-please --renew
```
config nginx
```
ssl_certificate    /path/*.domain/fullchain.cer;
ssl_certificate_key    /path/*.domain/*.domain.key;
```
