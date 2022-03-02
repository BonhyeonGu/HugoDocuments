---


---

<hr>
<h2 id="title-도커-파일description-도커에서-컨테이너를-생성해-주는-도커-파일의-간단한-예제입니다.date-2022-01-02t0221440900categories-도커tags-containerimage-thum.pngdraft-false">title: “도커 파일”<br>
description: “도커에서 컨테이너를 생성해 주는 도커 파일의 간단한 예제입니다.”<br>
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

<span class="token comment">#작성자 정보, 필수는 아님, 옛날에는 MAINTAINER을 사용했으나 대체됨</span>
<span class="token keyword">LABEL</span> email=<span class="token string">"tayasriel@gmail.com"</span>
<span class="token keyword">LABEL</span> name=<span class="token string">"BonhyeonGu"</span>

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
<span class="token keyword">LABEL</span> email=<span class="token string">"tayasriel@gmail.com"</span>
<span class="token keyword">LABEL</span> name=<span class="token string">"BonhyeonGu"</span>

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
<h2 id="도커-컴포즈-도커-파일의-조합">도커 컴포즈, 도커 파일의 조합</h2>
<h3 id="안내">안내</h3>
<p>이런 방법으로 컴공에 새로 입문하시는 분들께 개발환경을 배포할 수 있을 것 같습니다.</p>
<h3 id="파이썬-인터프리터">파이썬 인터프리터</h3>
<p>도커파일</p>
<pre class=" language-docker"><code class="prism  language-docker"><span class="token keyword">FROM</span> ubuntu
<span class="token keyword">LABEL</span> email=<span class="token string">"tayasriel@gmail.com"</span>
<span class="token keyword">LABEL</span> name=<span class="token string">"BonhyeonGu"</span>

<span class="token keyword">RUN</span>  apt<span class="token punctuation">-</span>get update <span class="token punctuation">-</span>y &amp;&amp; \
 apt<span class="token punctuation">-</span>get install <span class="token punctuation">-</span>y python3<span class="token punctuation">-</span>pip python3<span class="token punctuation">-</span>dev

<span class="token comment">#mkdir 이 필요할 수 있다.</span>
<span class="token keyword">WORKDIR</span>  /app
<span class="token keyword">RUN</span> pip install <span class="token punctuation">-</span><span class="token punctuation">-</span>no<span class="token punctuation">-</span>cache<span class="token punctuation">-</span>dir <span class="token punctuation">-</span><span class="token punctuation">-</span>upgrade pip &amp;&amp; \
    pip install numpy
    <span class="token comment">#경우에 따라서 pip 모두 업그레이드 받는건 피해야겠다. 꽤 오래걸림</span>

<span class="token comment">#명령어 실행, RUN과 다르게 컨테이너가 활성화 될 때 마다 실행된다.</span>
<span class="token keyword">ENTRYPOINT</span>  <span class="token punctuation">[</span> <span class="token string">"python3"</span> <span class="token punctuation">]</span>

<span class="token comment">#명령어 실행, ENTRYPOINT 와 비슷하지만, 만약 도커 컨테이너 실행과 함께</span>
<span class="token comment">#인수가 제공되면 그것으로 대체된다.</span>
<span class="token keyword">CMD</span>  <span class="token punctuation">[</span> <span class="token string">"app.py"</span> <span class="token punctuation">]</span>

<span class="token comment">## 즉 이 형태는 컨테이너 start와 함께, 파이썬이 실행되고 컨테이너가 stop되는 형식</span>
</code></pre>
<p>컴포즈</p>
<pre class=" language-docker"><code class="prism  language-docker">version<span class="token punctuation">:</span> <span class="token string">"3"</span>
services<span class="token punctuation">:</span>
 test1<span class="token punctuation">:</span>
  build<span class="token punctuation">:</span> .
  volumes<span class="token punctuation">:</span>
   <span class="token punctuation">-</span> .<span class="token punctuation">:</span>/app
</code></pre>
<p>이후</p>
<pre class=" language-docker"><code class="prism  language-docker">docker<span class="token punctuation">-</span>compose up &amp;&amp; docker<span class="token punctuation">-</span>compose rm <span class="token punctuation">-</span>f
</code></pre>
<p>로 쉬운 실행/삭제 가능</p>
<h3 id="파이썬-리모트-전용">파이썬 리모트 전용</h3>
<p>도커파일</p>
<pre class=" language-docker"><code class="prism  language-docker"><span class="token keyword">FROM</span> ubuntu
<span class="token keyword">LABEL</span> email=<span class="token string">"tayasriel@gmail.com"</span>
<span class="token keyword">LABEL</span> name=<span class="token string">"BonhyeonGu"</span>

<span class="token comment">#언어 설정, ENV 자체는 도커 이미지 환경변수를 조작한다.</span>
<span class="token keyword">ENV</span> LC_ALL=C.UTF<span class="token punctuation">-</span>8
<span class="token keyword">ENV</span> LANG=C.UTF<span class="token punctuation">-</span>8
<span class="token keyword">RUN</span>  apt<span class="token punctuation">-</span>get update <span class="token punctuation">-</span>y &amp;&amp; \
 apt<span class="token punctuation">-</span>get install <span class="token punctuation">-</span>y python3<span class="token punctuation">-</span>pip python3<span class="token punctuation">-</span>dev

<span class="token comment">#vscode remote시 초기 경로를 root로 잡는다.</span>
<span class="token keyword">WORKDIR</span>  /root
<span class="token keyword">RUN</span> pip install <span class="token punctuation">-</span><span class="token punctuation">-</span>no<span class="token punctuation">-</span>cache<span class="token punctuation">-</span>dir <span class="token punctuation">-</span><span class="token punctuation">-</span>upgrade pip &amp;&amp; \
    pip install numpy
</code></pre>
<p>컴포즈</p>
<pre class=" language-docker"><code class="prism  language-docker">version<span class="token punctuation">:</span> <span class="token string">"3"</span>
services<span class="token punctuation">:</span>
 test2<span class="token punctuation">:</span>
  <span class="token comment">#it에 효과랑 같다는데 원하는 대로 실행되지 않았다.</span>
  <span class="token comment">#stdin_open: true # docker run -i</span>
  tty<span class="token punctuation">:</span> true        <span class="token comment"># docker run -t</span>
  build<span class="token punctuation">:</span> .
  volumes<span class="token punctuation">:</span>
   <span class="token punctuation">-</span> .<span class="token punctuation">:</span>/root
</code></pre>
<p>이후</p>
<pre class=" language-docker"><code class="prism  language-docker">docker<span class="token punctuation">-</span>compose up <span class="token punctuation">-</span>d
code .
그리고 리모트<span class="token punctuation">,</span> 다쓰면 이후로
docker<span class="token punctuation">-</span>compose down
</code></pre>

