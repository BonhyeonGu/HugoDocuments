---


---

<hr>
<h2 id="title-rdp--문제description-rdp-에서-겪었던-문제를-해결하는-것에-대한-서술입니다.date-2021-12-31t1138570900categories-윈도우tags-rdp-shellimage-thum.pngdraft-false">title: “RDP  문제”<br>
description: “RDP 에서 겪었던 문제를 해결하는 것에 대한 서술입니다.”<br>
date: 2021-12-31T11:38:57+09:00<br>
categories: [“윈도우”]<br>
tags: [“rdp”, “shell”]<br>
image: “thum.png”<br>
draft: false</h2>
<h2 id="설명">설명</h2>
<h3 id="문제">문제</h3>
<p>윈도우즈, 윈도우즈 서버 운영체제를 사용할 때 정책을 사용해야 하는 이유로<br>
반드시 공식 지원하는 'RDP’를 사용해야 할 때가 있습니다.</p>
<p>해당 프로그램은 대부분의 기능을 지원하지만 의외의 문제점이 있습니다.<br>
바로 SSH, 원격 터미널과 흡사한 부분이 있는 것입니다.<br>
<strong>RDP 은 유저가 연결을 끊으면 로그오프되어 Foreground 는 당연하고 Background마저 설정해 놓지 않으면 닫히게 됩니다.</strong></p>
<p>한번은 윈도우 환경에서만 지원하는 GUI Foreground 프로세스를 지속적으로 가동시킬 목적으로 AWS에서  Windows Server를 구입하여 사용한 적이 있었습니다.<br>
하지만 위와 같은 이유로 곤혹을 겪었습니다.</p>
<h3 id="해결">해결</h3>
<p>해결방법은 RDP를 마치고 싶을 때 연결 종료가 아닌<br>
작업중인 호스트에서 자기 자신한테 RDP를 연결 시키는 것입니다.<br>
이러면 RDP 가 끝나지 않았으니 작업을 멈추지 않으며,<br>
다른 사람이 연결을 신청했으니 현제 연결중인 클라이언트는 끊기게 됩니다.<br>
나중에 알게됐지만 이 방법은 여러 곳에서 많이 사용중 이였습니다.</p>
<h2 id="powershell-코드">PowerShell 코드</h2>
<h3 id="코드">코드</h3>
<pre class=" language-ps1"><code class="prism  language-ps1"><span class="token variable">$ID</span>
<span class="token variable">$FIND</span> = 0
<span class="token variable">$LINES</span> = query session

<span class="token keyword">foreach</span> <span class="token punctuation">(</span><span class="token variable">$LINE</span> in <span class="token variable">$LINES</span><span class="token punctuation">)</span>
<span class="token punctuation">{</span>
	<span class="token variable">$WORDS</span> = <span class="token variable">$LINE</span><span class="token punctuation">.</span>split<span class="token punctuation">(</span><span class="token punctuation">)</span> <span class="token punctuation">|</span> where <span class="token punctuation">{</span><span class="token variable">$_</span><span class="token punctuation">}</span>
	<span class="token keyword">foreach</span> <span class="token punctuation">(</span><span class="token variable">$WORD</span> in <span class="token variable">$WORDS</span><span class="token punctuation">)</span>
	<span class="token punctuation">{</span>
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token variable">$WORD</span> <span class="token operator">-eq</span> <span class="token string">" "</span><span class="token punctuation">)</span>
		<span class="token punctuation">{</span>
			<span class="token function">echo</span> <span class="token string">"!"</span>
			<span class="token keyword">continue</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token variable">$FIND</span> <span class="token operator">-eq</span> 1<span class="token punctuation">)</span>
		<span class="token punctuation">{</span>
			<span class="token function">echo</span> <span class="token string">"!"</span>
			<span class="token keyword">continue</span>
		<span class="token punctuation">}</span>
		<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token variable">$WORD</span> <span class="token operator">-eq</span> <span class="token string">"user01"</span><span class="token punctuation">)</span>
		<span class="token punctuation">{</span>
			<span class="token variable">$FIND</span> = 1
		<span class="token punctuation">}</span>
	<span class="token punctuation">}</span>
	<span class="token keyword">if</span><span class="token punctuation">(</span><span class="token variable">$FIND</span> <span class="token operator">-eq</span> 1<span class="token punctuation">)</span>
	<span class="token punctuation">{</span>
		<span class="token keyword">break</span>
	<span class="token punctuation">}</span>
<span class="token punctuation">}</span>

<span class="token variable">$CMD</span> = <span class="token string">"tscon <span class="token variable">$ID</span> /dest:console /password:!!!!!!!!"</span>
<span class="token function">iex</span> <span class="token variable">$CMD</span>
</code></pre>
<h3 id="설명-1">설명</h3>
<p>먼저 현재 진행중인 RDP를 찾아냅니다.<br>
이후 해당 세션을 tscon으로 루프백 연결시키게 만듭니다.</p>
<h2 id="파워쉘-코드를-실행하는-배치파일">파워쉘 코드를 실행하는 배치파일</h2>
<h3 id="코드-1">코드</h3>
<pre class=" language-bat"><code class="prism  language-bat">runas /user:localhost\administrator "powershell.exe -noprofile -executionpolicy bypass -file "POWER경로""
</code></pre>
<h3 id="설명-2">설명</h3>
<p>또 다른 문제로, 해당 Powershell 코드는 관리자 권한이 필요한데<br>
세션에 접속한 사용자는 관리자가 아니였습니다. 이를 위해 runas를 사용하여<br>
관리자 권한을 통하여 코드를 실행하게 만들었습니다.</p>

