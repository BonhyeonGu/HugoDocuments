---


---

<hr>
<h2 id="title-도커-이미지-파일description-도커에서-컨테이너를-생성해-주는-이미지파일의-간단한-예제입니다.date-2022-01-02t0221440900categories-도커tags-containerimage-thum.pngdraft-false">title: “도커 이미지 파일”<br>
description: “도커에서 컨테이너를 생성해 주는 이미지파일의 간단한 예제입니다.”<br>
date: 2022-01-02T02:21:44+09:00<br>
categories: [“도커”]<br>
tags: [“container”]<br>
image: “thum.png”<br>
draft: false</h2>
<h2 id="간단한-형식">간단한 형식</h2>
<h3 id="운영체제">운영체제</h3>
<pre class=" language-docker"><code class="prism  language-docker"><span class="token comment">#원본이 될 이미지, https://hub.docker.com/ 참고</span>
<span class="token comment">#버전을 기록하지 않으면 최신 버전을 사용하게 된다.</span>
<span class="token keyword">FROM</span> ubuntu<span class="token punctuation">:</span>16.04

<span class="token comment">#작성자 정보, 필수는 아님</span>
<span class="token keyword">MAINTAINER</span> <span class="token string">"tayasriel.@gmail.com"</span>

<span class="token comment">#언어 설정, ENV 자체는 도커 이미지 환경변수를 조작한다.</span>
<span class="token keyword">ENV</span> LC_ALL=C.UTF<span class="token punctuation">-</span>8
<span class="token keyword">ENV</span> LANG=C.UTF<span class="token punctuation">-</span>8

<span class="token comment">#명령어, 물음이 오는것을 피해야 진행이 가능하다.</span>
<span class="token keyword">RUN</span> apt<span class="token punctuation">-</span>get update <span class="token punctuation">-</span>y &amp;&amp; \
 apt<span class="token punctuation">-</span>get install <span class="token punctuation">-</span>y nano
 
<span class="token comment">#노출될 포트, 주의할 점은 이게 포트포워딩을 해주는 것이 아님</span>
<span class="token keyword">EXPOSE</span> 5000<span class="token punctuation">,</span> 5001
</code></pre>
<h3 id="인터프리터">인터프리터</h3>
<pre class=" language-docker"><code class="prism  language-docker"><span class="token keyword">FROM</span> ubuntu
<span class="token keyword">MAINTAINER</span> <span class="token string">"tayasriel.@gmail.com"</span>

<span class="token keyword">RUN</span>  apt<span class="token punctuation">-</span>get update <span class="token punctuation">-</span>y &amp;&amp; \
 apt<span class="token punctuation">-</span>get install <span class="token punctuation">-</span>y python<span class="token punctuation">-</span>pip python<span class="token punctuation">-</span>dev

<span class="token comment">#호스트 경로 .의 파일들을 컨테이너의 경로 /app으로 복사시킨다.</span>
<span class="token keyword">COPY</span>  . /app

<span class="token comment">#컨테이너가 실행되면 활성화 될 경로, cd /app 과 동일하다.</span>
<span class="token keyword">WORKDIR</span>  /app
<span class="token keyword">RUN</span>  pip install <span class="token punctuation">-</span>r requirements.txt

<span class="token comment">#명령어 실행, RUN과 다르게 컨테이너가 활성화 될 때 마다 실행된다.</span>
<span class="token keyword">ENTRYPOINT</span>  <span class="token punctuation">[</span> <span class="token string">"python"</span> <span class="token punctuation">]</span>

<span class="token comment">#명령어 실행, ENTRYPOINT 와 비슷하지만, 만약 도커 컨테이너 실행과 함께</span>
<span class="token comment">#인수가 제공되면 그것으로 대체된다.</span>
<span class="token keyword">CMD</span>  <span class="token punctuation">[</span> <span class="token string">"app.py"</span> <span class="token punctuation">]</span>

<span class="token comment">## 즉 이 형태는 컨테이너 start와 함께, 파이썬이 실행되고 컨테이너가 stop되는 형식</span>
</code></pre>
<h2 id="응용">응용</h2>
<h3 id="ide-remote">IDE Remote</h3>
<pre class=" language-docker"><code class="prism  language-docker"><span class="token keyword">FROM</span> ubuntu
<span class="token keyword">MAINTAINER</span> <span class="token string">"tayasriel.@gmail.com"</span>

<span class="token keyword">ENV</span> LC_ALL=C.UTF<span class="token punctuation">-</span>8
<span class="token keyword">ENV</span> LANG=C.UTF<span class="token punctuation">-</span>8

<span class="token keyword">RUN</span>  apt<span class="token punctuation">-</span>get update <span class="token punctuation">-</span>y &amp;&amp; \
 apt<span class="token punctuation">-</span>get install <span class="token punctuation">-</span>y python<span class="token punctuation">-</span>pip python<span class="token punctuation">-</span>dev

<span class="token comment">#경로는 자유</span>
<span class="token keyword">COPY</span>  tesk /var/tesk
<span class="token keyword">WORKDIR</span>  /var/tesk

<span class="token keyword">RUN</span>  pip install <span class="token punctuation">-</span>r requirements.txt

<span class="token comment">#파이썬 설정</span>
<span class="token keyword">ENV</span> PYTHONUNBUFFERED=TRUE
<span class="token keyword">ENV</span> PYTHONDONTWRITEBYTECODE=TRUE
<span class="token keyword">ENV</span> PATH=<span class="token string">"/var/task:${PATH}"</span>

<span class="token comment">#해당 컨테이너 안에서 IDE가 동작하는 방식이기 때문에</span>
<span class="token comment">#인자를 받거나 기타 명령어를 수행할 필요 없다.</span>
</code></pre>
<h3 id="서버">서버</h3>

