### E-001 재귀 조건

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits((\texttt{?(R)}R_1\texttt{|}R_2)) &= \mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{incall}}\nolimits;R_1,R_2),\\
\mathcal L((\texttt{?(R)}R_1\texttt{|}R_2)) &= \mathcal L(R_2),\\
(\texttt{?(R)}R_1\texttt{|}R_2) &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{incall}}\nolimits;R_1,R_2).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta((\texttt{?(R)}R_1\texttt{|}R_2)),\\
\mathop{\mathrm{brif}}\nolimits(p;R_1,R_2)\text{는 폭 0 술어 }p\text{가 참이면 }R_1,\text{ 거짓이면 }R_2\text{를 실행},\\
\mathop{\mathrm{incall}}\nolimits\iff\text{현재 실행이 어떤 호출에 의해 재진입된 상태},\\
[\mathcal L]\qquad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** $`\mathop{\mathrm{incall}}\nolimits`$은 문자를 소비하지 않는다. 호출이 없는 패턴에서는 항상 거짓이므로 단독 전체 매치의 언어는 $`\mathcal L(R_2)`$다. 예를 들어 `(?(R)a|b)c`는 `bc`와 언어가 같다. 기존 연산으로 만든 식은 같은 위치·같은 입력에서 항상 같은 판정을 내리므로, 호출 레벨에 따라 가지가 갈리는 패턴 — `(x(?(R)a|b)(?1)?)`는 바깥 레벨에서 `xb`꼴, 호출 레벨에서 `xa`꼴 — 과 매치 동치가 될 수 없다. PCRE2에서는 원래 구문으로 표기한다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#conditions)

### E-002 재귀 조건 (번호)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{incall}}\nolimits(1);R_1,R_2)\,\mathop{\mathrm{cap}}\nolimits(R_3),\\
\mathcal L(P) &= \mathcal L(R_2)\,\mathcal L(R_3),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(\texttt{?(R}n\texttt{)}R_1\texttt{|}R_2)(R_3),\qquad
\mathop{\mathrm{incall}}\nolimits(g)\iff\text{그룹 }g\text{의 호출 안},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R_1,R_2,R_3\in\mathcal E_0.
\end{gathered}
```

**근거.** E-001과 달리 조건이 가리키는 번호의 그룹이 존재해야 컴파일된다. `(?(R1)a|b)`는 단독으로 컴파일되지 않고 `(?(R1)a|b)(x)`는 유효하다. 술어만 다를 뿐 분기 구조는 E-001의 $`\mathop{\mathrm{brif}}\nolimits`$를 그대로 사용한다. 호출 없는 패턴에서 조건은 항상 거짓이다. 예를 들어 `(?(R1)a|b)(x)`는 `bx`와 언어가 같다.

### E-003 재귀 조건 (이름)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{incall}}\nolimits(\mathop{\mathrm{res}}\nolimits(m));R_1,R_2)\,\mathop{\mathrm{cap}}\nolimits_m(R_3),\\
\mathcal L(P) &= \mathcal L(R_2)\,\mathcal L(R_3),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(\texttt{?(R\&}m\texttt{)}R_1\texttt{|}R_2)(\texttt{?<}m\texttt{>}R_3),\qquad m\in\mathrm{Name},\\
\mathop{\mathrm{res}}\nolimits(m)=N_\theta(P)\text{에서 }m\text{에 대응하는 그룹 번호},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R_1,R_2,R_3\in\mathcal E_0.
\end{gathered}
```

**근거.** 이름을 $`\mathop{\mathrm{res}}\nolimits`$로 번호로 해석한 뒤 E-002와 같다. 예를 들어 `(?(R&w)a|b)(?<w>x)`는 `bx`와 언어가 같다.

### E-004 서브루틴 정의

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits((\texttt{?(DEFINE)}(\texttt{?<}m\texttt{>}R))) &= \mathop{\mathrm{zero}}\nolimits(\mathop{\mathrm{cap}}\nolimits_m(R)),\\
\mathcal L((\texttt{?(DEFINE)}(\texttt{?<}m\texttt{>}R))) &= \{\varepsilon\},\\
(\texttt{?(DEFINE)}(\texttt{?<}m\texttt{>}R)) &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{zero}}\nolimits(\mathop{\mathrm{cap}}\nolimits_m(R)).
\end{aligned}
```

**조건.**

```math
m\in\mathrm{Name},\qquad k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(\cdot).
```

**근거.** 새 연산이 필요 없다. DEFINE 조건은 항상 거짓이라 본문을 실행하지 않되 호출 가능한 그룹 정의는 남기는데, 이는 B-017의 $`\mathop{\mathrm{zero}}\nolimits`$가 하는 일과 같다. 예를 들어 `(?(DEFINE)(?<x>ab))(?&x)`와 `(?<x>ab){0}(?&x)`는 매치 동치이며, 재귀 정의 `(?<x>a(?&x)?b)`에서도 성립한다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#conditions)

### E-005 전체 재귀

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= R_1(\mathop{\mathrm{call}}\nolimits(0)\mid\varepsilon)R_2,\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=R_1\texttt{(?R)?}R_2,\qquad
\mathop{\mathrm{call}}\nolimits(g)\text{는 그룹 }g\text{의 패턴을 현재 위치에서 실행}\ (g=0\text{은 전체 패턴}),\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
\text{호출 그래프에 출구 존재},\qquad
[\mathcal L]\quad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** $`\mathcal L(P)`$는 비정규이므로 — 예를 들어 `a(?R)?b`의 언어는 $`\{a^nb^n\mid n\ge1\}`$ — 기존 연산으로는 언어 동치조차 성립하지 않는다. 호출은 성공한 뒤 내부로 백트래킹하지 않는다. "전체 패턴"은 컴파일 단위 기준이므로, 전체 매치를 `\A(?:□)\z` 래핑으로 구현하면 재귀가 `\A`까지 재진입해 깨진다: `\A(?:a(?R)?b)\z`는 `aabb`에 실패한다. `(?R)`을 담은 패턴은 래핑 대신 `PCRE2_ANCHORED`·`PCRE2_ENDANCHORED`로 전체 매치를 강제해야 한다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#recursion)

### E-006 전체 재귀 (번호 0 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= R_1(\mathop{\mathrm{call}}\nolimits(0)\mid\varepsilon)R_2,\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=R_1\texttt{(?0)?}R_2`$, 나머지는 E-005와 같다.

**근거.** `(?0)`은 `(?R)`과 같은 전체 재귀를 생성한다. E-005의 $`\mathop{\mathrm{call}}\nolimits(0)`$을 그대로 사용하며, 래핑 주의도 동일하다.

### E-007 서브패턴 호출 (절대 번호)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1)\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(R_1\texttt{(?1)?}R_2),\qquad k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\\
\text{호출 그래프에 출구 존재},\qquad
[\mathcal L]\quad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** 호출은 그룹의 **패턴**을 재실행한다 — 캡처된 문자열을 비교하는 F-002의 $`\mathop{\mathrm{ref}}\nolimits`$와 다르다. 호출이 그룹 안이면 재귀이고 $`\mathcal L(P)`$는 E-005와 같은 논거로 비정규다. 그룹 밖이면 subroutine이며 $`\mathcal L((R)\texttt{(?1)})=\{uv\mid u,v\in\mathcal L(R)\}`$이다. 예를 들어 `(ab)(?1)`은 `abab`와 언어가 같다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#subpatternsassubroutines)

### E-008 서브패턴 호출 (상대 전방)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{call}}\nolimits(1)\,\mathop{\mathrm{cap}}\nolimits(R),\\
\mathcal L(P) &= \{uv\mid u,v\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=\texttt{(?+1)}(R),\qquad
\mathop{\mathrm{res}}\nolimits_C(+n)=\text{삽입 위치 뒤에서 }n\text{번째로 열리는 그룹의 번호},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R\in\mathcal E_0.
\end{gathered}
```

**근거.** $`\texttt{(?+}n\texttt{)}`$은 $`\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits_C(+n))`$이다. 호출을 포함하는 그룹은 호출보다 먼저 열리므로 이 표기로는 자기 재귀를 쓸 수 없고, 전방 subroutine이 유일한 용법이다. 예를 들어 `(?+1)(ab)`는 `abab`와, `(?+1){2}(ab)`는 `ababab`와 언어가 같다.

### E-009 서브패턴 호출 (상대 역방향)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1)\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1\texttt{(?-1)?}R_2)`$, $`\mathop{\mathrm{res}}\nolimits_C(-n)`$은 삽입 위치 앞에서 $`n`$번째로 열린 그룹의 번호이고, 나머지는 E-007과 같다.

**근거.** $`\texttt{(?-}n\texttt{)}`$은 $`\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits_C(-n))`$이다. 자기 그룹 안에서 쓰면 $`\mathop{\mathrm{res}}\nolimits_C(-1)`$이 자기 그룹이 되어 재귀가 된다. E-007의 모든 성질을 공유한다. 예를 들어 `(b|a(?-1))`은 `a*b`와 언어가 같다.

### E-010 서브패턴 호출 (이름, Perl 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R_1(\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits(m))\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R_1\texttt{(?\&}m\texttt{)?}R_2)`$, $`m\in\mathrm{Name}`$, 나머지는 E-007과 같다.

**근거.** $`\texttt{(?\&}m\texttt{)}`$은 이름을 해석한 번호에 대한 E-007의 $`\mathop{\mathrm{call}}\nolimits`$을 그대로 사용한다. 예를 들어 `(?<w>b|a(?&w))`는 `a*b`와 언어가 같다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#subpatternsassubroutines)

### E-011 서브패턴 호출 (이름, Python 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R_1(\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits(m))\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?P<}m\texttt{>}R_1\texttt{(?P>}m\texttt{)?}R_2)`$, 나머지는 E-010과 같다.

**근거.** $`\texttt{(?P>}m\texttt{)}`$은 E-010과 같은 호출을 생성한다. B-008이 $`\mathop{\mathrm{cap}}\nolimits_m`$을 재사용한 것과 같다.

### E-012 서브패턴 호출 (Oniguruma 꺾쇠 번호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1)\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1\texttt{\textbackslash g<1>?}R_2)`$, 나머지는 E-007과 같다.

**근거.** $`\texttt{\textbackslash g<}n\texttt{>}`$은 E-007과 같은 호출이다 (백레퍼런스 표기 $`\texttt{\textbackslash g\{\}}`$와 다르다). 예를 들어 `(ab)\g<1>`은 `abab`와 언어가 같다.

### E-013 서브패턴 호출 (Oniguruma 따옴표 번호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1)\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1\texttt{\textbackslash g'1'?}R_2)`$, 나머지는 E-007과 같다.

**근거.** E-012와 같은 호출의 따옴표 표기다.

### E-014 서브패턴 호출 (Oniguruma 꺾쇠 이름 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R_1(\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits(m))\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R_1\texttt{\textbackslash g<}m\texttt{>?}R_2)`$, 나머지는 E-010과 같다.

**근거.** E-010과 같은 이름 호출의 Oniguruma 표기다.

### E-015 서브패턴 호출 (Oniguruma 따옴표 이름 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R_1(\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits(m))\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R_1\texttt{\textbackslash g'}m\texttt{'?}R_2)`$, 나머지는 E-010과 같다.

**근거.** E-014의 따옴표 표기다.

### E-016 서브패턴 호출 (Oniguruma 상대 전방 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{call}}\nolimits(1)\,\mathop{\mathrm{cap}}\nolimits(R),\\
\mathcal L(P) &= \{uv\mid u,v\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=\texttt{\textbackslash g<+1>}(R)`$, 나머지는 E-008과 같다.

**근거.** $`\pm n`$ 상대 표기는 PCRE2 확장이다. E-008과 같이 자기 재귀가 불가능하며 전방 subroutine 전용이다. 예를 들어 `\g<+1>(ab)`는 `abab`와 언어가 같다.

### E-017 서브패턴 호출 (Oniguruma 상대 역방향 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1)\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1\texttt{\textbackslash g<-1>?}R_2)`$, 나머지는 E-009와 같다.

**근거.** E-009와 같은 상대 역방향 호출의 Oniguruma 표기다.

### E-018 반환 리스트 호출 (전체 재귀)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= R_1(\mathop{\mathrm{call}}\nolimits(0;\mathit{list})\mid\varepsilon)R_2,\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=R_1\texttt{(?R(}\mathit{list}\texttt{))?}R_2,\qquad
\mathop{\mathrm{call}}\nolimits(g;\mathit{list})\text{는 }\mathop{\mathrm{call}}\nolimits(g)\text{ 실행 후 }\mathit{list}\text{의 캡처를 호출자에게 반환},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** $`\mathit{list}`$는 소비 문자를 바꾸지 않고 호출 종료 후 어떤 캡처가 보이는지만 바꾼다. 따라서 호출 뒤에서 $`\mathit{list}`$의 캡처를 참조하지 않는 문맥에서는 리스트 없는 호출과 매치 동치다. 이 문맥 조건은 생략할 수 없다: `((a|b))x(?1(2))\2`는 `axbb`에 매치하고, 리스트를 뗀 `((a|b))x(?1)\2`는 `axba`에 매치한다. 언어와 래핑 주의는 E-005와 같다.

### E-019 반환 리스트 호출 (번호)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1(\mathop{\mathrm{call}}\nolimits(1;\mathit{list})\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1\texttt{(?1(}\mathit{list}\texttt{))?}R_2)`$, 나머지는 E-018과 같다.

**근거.** E-007의 호출에 E-018의 반환 리스트를 결합한 것이다.

### E-020 반환 리스트 호출 (이름)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R_1(\mathop{\mathrm{call}}\nolimits(\mathop{\mathrm{res}}\nolimits(m);\mathit{list})\mid\varepsilon)R_2),\\
\mathcal L(P) &= \bigcup_{j\ge 1}\mathcal L(R_1)^j\,\mathcal L(R_2)^j,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R_1\texttt{(?\&}m\texttt{(}\mathit{list}\texttt{))?}R_2)`$, 나머지는 E-018과 같다.

**근거.** E-010의 이름 호출에 E-018의 반환 리스트를 결합한 것이다.

### F-001 역참조 (\ddd 해석)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(R)\texttt{\textbackslash ddd},\qquad
\texttt{\textbackslash ddd}\text{가 Perl 규칙으로 8진 이스케이프가 아닌 역참조로 해석됨},\\
\mathop{\mathrm{ref}}\nolimits(g)\text{는 그룹 }g\text{가 캡처한 문자열과 글자 단위 비교. }\mathsf{Unset}\text{이면 실패},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R\in\mathcal E_0,\ \mathop{\mathrm{slots}}\nolimits(R)=0.
\end{gathered}
```

**근거.** $`\mathop{\mathrm{ref}}\nolimits`$는 캡처된 **문자열**을 비교한다 — 패턴을 재실행하는 E-007의 $`\mathop{\mathrm{call}}\nolimits`$과 다르다. $`\mathcal L((R_0)\texttt{\textbackslash 1})`$은 $`R_0=\texttt{ab*}`$일 때 $`\{ab^kab^k\mid k\ge0\}`$으로 비정규이므로 기존 연산으로 표현할 수 없다. 참고로 $`R_0=\texttt{a*}`$의 언어는 $`\{a^{2n}\}`$으로 정규라 불가 근거로는 부적합하다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#backreferences)

### F-002 역참조 (절대 번호)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{\textbackslash}n`$, 나머지는 F-001과 같다.

**근거.** F-001과 같은 $`\mathop{\mathrm{ref}}\nolimits`$이며, 해석 모호성이 없는 표기다.

### F-003 역참조 (\g 번호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{\textbackslash g}n`$, 나머지는 F-001과 같다.

**근거.** F-002와 같은 $`\mathop{\mathrm{ref}}\nolimits(n)`$의 표기 변형이다.

### F-004 역참조 (\g{} 번호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{\textbackslash g\{}n\texttt{\}}`$, 나머지는 F-001과 같다.

**근거.** F-003의 중괄호 표기다.

### F-005 역참조 (상대 역방향)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{\textbackslash g-}n`$, $`\mathop{\mathrm{res}}\nolimits_C(-n)`$은 E-009와 같다.

**근거.** $`\texttt{\textbackslash g-}n`$은 $`\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits_C(-n))`$이다. 해석 결과가 앞선 그룹이므로 F-002와 성질이 같다.

### F-006 역참조 (상대 역방향, 중괄호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R)\,\mathop{\mathrm{ref}}\nolimits(1),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{\textbackslash g\{-}n\texttt{\}}`$, 나머지는 F-005와 같다.

**근거.** F-005의 중괄호 표기다.

### F-007 역참조 (상대 전방)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{ref}}\nolimits(1)\,\mathop{\mathrm{cap}}\nolimits(R),\\
\mathcal L(P) &= \varnothing,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=\texttt{\textbackslash g+}n\,(R)`$, $`\mathop{\mathrm{res}}\nolimits_C(+n)`$은 E-008과 같다.

**근거.** $`\texttt{\textbackslash g+}n`$은 $`\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits_C(+n))`$이다. 반복 밖에서는 참조 평가 시점에 대상 그룹이 항상 $`\mathsf{Unset}`$이라 실패하므로 언어가 $`\varnothing`$이다 — C-001과 같이 기존 연산 밖의 빈 언어다. 반복 안에서는 이전 바퀴의 캡처를 읽어 비자명해진다 (F-014 참조). 전방 참조가 역참조에서는 가능하고 자기 재귀가 호출에서 불가능한 것(E-008)은 같은 $`\mathop{\mathrm{res}}\nolimits(+n)`$ 해석의 두 귀결이다.

### F-008 역참조 (상대 전방, 중괄호 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{ref}}\nolimits(1)\,\mathop{\mathrm{cap}}\nolimits(R),\\
\mathcal L(P) &= \varnothing,\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=\texttt{\textbackslash g\{+}n\texttt{\}}(R)`$, 나머지는 F-007과 같다.

**근거.** F-007의 중괄호 표기다.

### F-009 역참조 (이름, \k<> 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R)\,\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits(m)),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{\textbackslash k<}m\texttt{>}`$, $`m\in\mathrm{Name}`$, 나머지는 F-001과 같다.

**근거.** 이름을 $`\mathop{\mathrm{res}}\nolimits`$로 해석한 뒤 F-002의 $`\mathop{\mathrm{ref}}\nolimits`$를 그대로 사용한다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#backreferences)

### F-010 역참조 (이름, \k'' 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R)\,\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits(m)),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{\textbackslash k'}m\texttt{'}`$, 나머지는 F-009와 같다.

**근거.** F-009의 따옴표 표기다.

### F-011 역참조 (이름, \g{} 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R)\,\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits(m)),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{\textbackslash g\{}m\texttt{\}}`$, 나머지는 F-009와 같다.

**근거.** F-009와 같은 이름 참조의 Perl \g 표기다.

### F-012 역참조 (이름, \k{} 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R)\,\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits(m)),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{\textbackslash k\{}m\texttt{\}}`$, 나머지는 F-009와 같다.

**근거.** F-009와 같은 이름 참조의 .NET 표기다.

### F-013 역참조 (이름, (?P=) 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits_m(R)\,\mathop{\mathrm{ref}}\nolimits(\mathop{\mathrm{res}}\nolimits(m)),\\
\mathcal L(P) &= \{ww\mid w\in\mathcal L(R)\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?P<}m\texttt{>}R)(\texttt{?P=}m)`$, 나머지는 F-009와 같다.

**근거.** F-009와 같은 이름 참조의 Python 표기다.

### F-014 반복 안 역참조

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits(R_1\mid R_2\,\mathop{\mathrm{ref}}\nolimits(1)))\texttt{+},\\
\mathcal L(P) &= \{w_1\cdots w_j\mid j\ge1,\ w_1\in\mathcal L(R_1),\ w_{i+1}\in\mathcal L(R_1)\cup\mathcal L(R_2)\{w_i\}\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(R_1\texttt{|}R_2\texttt{\textbackslash 1})\texttt{+},\qquad
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** 새 연산이 필요 없다 — $`\mathop{\mathrm{ref}}\nolimits`$는 참조 시점의 현재 캡처값을 읽으므로 반복을 자연히 포괄한다. 바퀴 $`i{+}1`$의 $`\mathop{\mathrm{ref}}\nolimits(1)`$은 바퀴 $`i`$가 남긴 값을 읽는다. 첫 바퀴는 참조가 $`\mathsf{Unset}`$이라 $`R_1`$ 가지만 가능하다. 바깥 `+`는 B-012의 전개 조건($`\mathop{\mathrm{slots}}\nolimits=0`$)을 위반하므로 전개하지 않는다. $`\mathcal L(P)`$는 비정규이므로 — `(a|b\1)+`의 원소는 `a` 또는 `b`·(직전 원소) — 유한 전개가 불가능하고, 포함 관계만 성립한다: $`\mathcal L(P)\subseteq\mathcal L((R_2^*R_1)\texttt{+})`$, $`\mathcal L(P)\supseteq\mathcal L(R_1\texttt{+})`$.

### F-015 부분문자열 스캔

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1)\,\mathop{\mathrm{scs}}\nolimits((1);R_2),\\
\mathcal L(P) &= \{w\in\mathcal L(R_1)\mid w\in\mathcal L(R_2)\Sigma^*\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(R_1)\texttt{(*scan\_substring:(1)}R_2\texttt{)},\qquad
\mathop{\mathrm{ver}}\nolimits(\theta)\ge 10.44,\\
\mathop{\mathrm{scs}}\nolimits(\mathit{list};R)\text{는 }\mathit{list}\text{ 중 처음 설정된 캡처의 문자열에 }R\text{을 그 맨 앞에 앵커드로 적용. 문자를 소비하지 않음},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R_1,R_2\in\mathcal E_0.
\end{gathered}
```

**근거.** 캡처값에 대한 검사라는 점에서 $`\mathop{\mathrm{ref}}\nolimits`$의 일반화다. 내부의 `$`·`\Z`·`\z`는 부분문자열의 끝을 보고 `^`는 리셋되지 않는다. `$`를 쓰면 정확히 떨어진다: $`(R)\texttt{(*scs:(1)}R\texttt{\$)}`$는 $`(R)`$과 매치 동치다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html)

### F-016 부분문자열 스캔 (약어 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{cap}}\nolimits(R_1)\,\mathop{\mathrm{scs}}\nolimits((1);R_2),\\
\mathcal L(P) &= \{w\in\mathcal L(R_1)\mid w\in\mathcal L(R_2)\Sigma^*\},\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R_1)\texttt{(*scs:(1)}R_2\texttt{)}`$, 나머지는 F-015와 같다.

**근거.** F-015의 약어 표기다.

### F-017 매치 조건 (절대 번호)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits(R)\mid\varepsilon)\,\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(1);R_1,R_2),\\
\mathcal L(P) &= \mathcal L(R)\,\mathcal L(R_1)\ \cup\ \mathcal L(R_2),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.**

```math
\begin{gathered}
P=(R)\texttt{?(?(1)}R_1\texttt{|}R_2\texttt{)},\qquad
\mathop{\mathrm{matched}}\nolimits(g)\iff\text{그룹 }g\text{의 캡처가 }\mathsf{Unset}\text{이 아님},\\
k\in\mathcal K,\qquad \mathcal C\subseteq\mathcal D_\theta(P),\qquad
[\mathcal L]\quad R,R_1,R_2\in\mathcal E_0,\ \varepsilon\notin\mathcal L(R).
\end{gathered}
```

**근거.** E-001의 $`\mathop{\mathrm{brif}}\nolimits`$에 술어만 $`\mathop{\mathrm{matched}}\nolimits`$로 바꾼 것이다. 그룹이 조건 앞·반복 밖이고 선택적이지 않으면 평가 시점에 항상 매치돼 $`R_1`$ 가지로 확정된다: `(a)(?(1)x|y)`는 `ax`와 언어가 같다. 그룹이 조건 뒤면 항상 $`\mathsf{Unset}`$이라 $`R_2`$ 가지로 확정된다: `(?(1)x|y)(a)`는 `ya`와 언어가 같다. [PCRE2 패턴 문서](https://www.pcre.org/current/doc/html/pcre2pattern.html#conditions)

### F-018 매치 조건 (상대 전방)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= \mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(1);R_1,R_2)\,\mathop{\mathrm{cap}}\nolimits(R),\\
\mathcal L(P) &= \mathcal L(R_2)\,\mathcal L(R),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=\texttt{(?(+1)}R_1\texttt{|}R_2\texttt{)}(R)`$, $`\mathop{\mathrm{res}}\nolimits_C(+n)`$은 E-008과 같고, 나머지는 F-017과 같다.

**근거.** $`\texttt{(?(+}n\texttt{))}`$은 $`\mathop{\mathrm{matched}}\nolimits(\mathop{\mathrm{res}}\nolimits_C(+n))`$이다. 해석 결과가 항상 조건 뒤의 그룹이라 평가 시점에 $`\mathsf{Unset}`$이고 조건은 항상 거짓이다. 예를 들어 `(?(+1)x|y)(a)`는 `ya`와 언어가 같다.

### F-019 매치 조건 (상대 역방향)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits(R)\mid\varepsilon)\,\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(1);R_1,R_2),\\
\mathcal L(P) &= \mathcal L(R)\,\mathcal L(R_1)\ \cup\ \mathcal L(R_2),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(R)\texttt{?(?(-1)}R_1\texttt{|}R_2\texttt{)}`$, $`\mathop{\mathrm{res}}\nolimits_C(-n)`$은 E-009와 같고, 나머지는 F-017과 같다.

**근거.** 해석 결과가 항상 조건 앞의 그룹이다. 그룹이 조건 뒤에만 있으면 해석할 그룹이 없어 컴파일되지 않는다.

### F-020 매치 조건 (이름)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits_m(R)\mid\varepsilon)\,\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(\mathop{\mathrm{res}}\nolimits(m));R_1,R_2),\\
\mathcal L(P) &= \mathcal L(R)\,\mathcal L(R_1)\ \cup\ \mathcal L(R_2),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{?(?(<}m\texttt{>)}R_1\texttt{|}R_2\texttt{)}`$, $`m\in\mathrm{Name}`$, 나머지는 F-017과 같다.

**근거.** 이름을 해석한 번호에 대한 F-017이다. 예를 들어 `(?<w>a)(?(<w>)x|y)`는 `ax`와 언어가 같다.

### F-021 매치 조건 (이름, 따옴표 표기)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits_m(R)\mid\varepsilon)\,\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(\mathop{\mathrm{res}}\nolimits(m));R_1,R_2),\\
\mathcal L(P) &= \mathcal L(R)\,\mathcal L(R_1)\ \cup\ \mathcal L(R_2),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{?(?('}m\texttt{')}R_1\texttt{|}R_2\texttt{)}`$, 나머지는 F-020과 같다.

**근거.** F-020의 따옴표 표기다.

### F-022 매치 조건 (이름, PCRE1 호환)

```math
\begin{aligned}
\mathop{\mathrm{basic}}\nolimits(P) &= (\mathop{\mathrm{cap}}\nolimits_m(R)\mid\varepsilon)\,\mathop{\mathrm{brif}}\nolimits(\mathop{\mathrm{matched}}\nolimits(\mathop{\mathrm{res}}\nolimits(m));R_1,R_2),\\
\mathcal L(P) &= \mathcal L(R)\,\mathcal L(R_1)\ \cup\ \mathcal L(R_2),\\
P &\equiv_{\theta,k}^{\mathcal C}\mathop{\mathrm{basic}}\nolimits(P).
\end{aligned}
```

**조건.** $`P=(\texttt{?<}m\texttt{>}R)\texttt{?(?(}m\texttt{)}R_1\texttt{|}R_2\texttt{)}`$, 나머지는 F-020과 같다. 단 `R` 뒤에 숫자가 오는 이름은 재귀 조건과 모호하다.

**근거.** F-020의 괄호 없는 표기다.