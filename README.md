markdown
# SMA++ <GMA Compiler>

**SMA++** — это ультра-быстрый язык программирования с закрытым исходным кодом (Closed Source), созданный для эры автоматизации и шифрования, при этом без потерь в производительности и безопасности памяти 

---

## ⚡ Ключевые особенности

1. **Скорость на уровне Ассемблера (Runtime > Всё):** Благодаря собственному бэкенду MLS, код компилируется напрямую в x86-64 подобный ассемблер, минуя стандартные библиотеки Си.
2. **Абсолютная безопасность («LARP»):** Встроенная обфускация, шифрование строк (XOR), защита от отладчиков (Anti-Debug) и механизмы самоуничтожения (Kill-Switch).
3. **Минимум букв:** 1 строка на SMA++ заменяет 2-5 строк на C/C++. Нет лишних скобок, точек с запятой и сложных типов.
4. **Автоматика вместо ручника:** GMA компилятор сам решает, какой тип данных использовать и какой самый эффективный на определённую задачу, автоматическое выделение и освобождение памяти и сами закрывающие ретурны и авто типизация.

---

## 🛠 Архитектура проекта

*   `smapp.py` — Интерпретатор и REPL для быстрого результата, иначе я в запoй уйду.
*   `cma7.py` — Главный GMA компонент который переводит в 12-13 ЯП.
*   `sma/lex.py` — Парсер и валидаторы анализ проeба.
*   `sma/gen_*.py` — Классические бэкенды (C, C++, Rust, Python, Go, Java и тд тп.).
*   `sma/mls/backend_x86.py` — **Собственный бэкенд MLS**. Генерирует блядский ассемблер.

---

## 🚀 Быстрый старт

### 1. Интерпретатор 
Идеально для тестирования логики и скриптов;

bash
python3 smapp.py examples/account_reg.sma


### 2. Компиляция в машинный код (MLS,Backend)

Собственная генерация блядского ассемблера 

bash
python3 cma7.py hello_mls.sma out --backend=mls --run


### 3. Классическая компиляция (13 языков)

Генерация кода в C, Rust, Python и другие.

---

## 📖 Синтаксис SMA++

Синтаксис строго зафиксирован
Он невероятно короткий и намеренно  запутанный, для ещё большей сложности  реверс инжиниринга


только печать текста

sma
s_Hello World


Переменные и печать

sma
x = 5
y = 10
s_Значение x: {x}, y: {y}


Условия и циклы

sma
e_x > 3
    s_Больше трех
t
    s_Меньше или равно

i = 0
g_i < 5
    s_Итерация {i}
    i = i + 1


Функции

sma
fn_add(a, b)
    h_a + b

result = fn_add(5, 3)
s_Результат: {result}


Однострочный формат

Разрешено использовать ; как разделитель

√sma
all
x=5;y=3;s_Сумма: {x+y}


часть governor логики 

sma
fn_should(p,t)=e_!cpu_can(p.pol);h_0|e_p.lim;p.lim=0;p.need=1;h_1|h_(t-p.time>=p.min_ns);fn_updn(p,t,n)=e_n>p.next&&t-p.time<p.up_ns;h_1|e_n<p.next&&t-p.time<p.dn_ns;h_1|h_0;fn_updn2(p,t,n)=e_p.next==n;h_0|e_fn_updn(p,t,n);p.cache=p.prev;h_0|p.next=n;p.time=t;h_1;fn_fast(p,t,n)=e_!fn_updn2(p,t,n);h_void|n=drv_fast(p.pol,n);e_!n;h_void|p.pol.cur=n;trace(n,p.pol.cpus);fn_def(p,t,n)=e_!fn_updn2(p,t,n);h_void|e_!p.wip;p.wip=1;irq_q(p.irq);fn_get(p,u,m)=f=arch_inv();p.pol.max|p.pol.cur;f=map(u,f,m);e_f==p.cache&&!p.need;h_p.next;p.need=0;p.prev=p.cache;p.cache=f;h_res(p.pol,f);fn_cpu_util(c,u,m,ty)=r=cpu_rq(c);e_rt(r)&&ty==FRQ;h_m|irq=util_irq(r);e_irq>=m;h_m|u+=util_rt(r);e_ty==FRQ;u=clamp(r,u)|dl=util_dl(r);e_u+dl>=m;h_m|e_ty==ENR;u+=dl|u=scale(u,irq,m)+irq;e_ty==FRQ;u+=bw_dl(r);h_min(m,u);fn_get_u(s)=r=cpu_rq(s.cpu);u=cfs(r);m=cap(r);s.max=m;s.bw=bw(r);u=boost(s.cpu,u);part(u,m,s.cpu);h_fn_cpu_util(s.cpu,u,m,FRQ);fn_iow_res(s,t,b)=e_t-s.last<=TICK;h_0|s.iw=b?s.min:0;s.pend=b;h_1;fn_iow(s,t,f)=b=f&IO;e_s.iw&&fn_iow_res(s,t,b);h_void|e_!b;h_void|e_s.pend;h_void|s.pend=1;e_s.iw;s.iw=min(s.iw<<1,1024);h_void|s.iw=s.min;fn_iow_ap(s,t,u,m)=e_!s.iw;h_u|e_fn_iow_res(s,t,0);h_u|e_!s.pend;s.iw>>=1;e_s.iw<s.min;s.iw=0;h_u|s.pend=0;h_max((s.iw*m)>>10,u);fn_single(h,t,f)=s=cont(h);p=s.p;fn_iow(s,t,f);s.last=t;ign(s,p);e_!fn_should(p,t);h_void|b=!p.need&&busy(s);u=fn_get_u(s);m=s.max;u=fn_iow_ap(s,t,u,m);nf=fn_get(p,u,m);e_b&&nf<p.next;nf=p.next;p.cache=p.prev;e_p.pol.fast;fn_fast(p,t,nf);t;lock(p.lock);fn_def(p,t,nf);unlock(p.lock);fn_shared(h,t,f)=s=cont(h);p=s.p;lock(p.lock);fn_iow(s,t,f);s.last=t;ign(s,p);e_fn_should(p,t);nf=fn_next_sh(s,t);e_p.pol.fast;fn_fast(p,t,nf);t;fn_def(p,t,nf);unlock(p.lock);fn_next_sh(s,t)=p=s.p;pol=p.pol;u=0;m=1;e_for_cpu(j,pol.rel,online);jc=per(j);ju=fn_get_u(jc);jm=jc.max;ju=fn_iow_ap(jc,t,ju,jm);e_ju*m>jm*u;u=ju;m=jm;h_fn_get(p,u,m)


---

## 🛡 Безопасность («LARP»)

Компилятор GMA (через бэкенд MLS) закладывает в бинарник несколько уровней защиты:

## 1. Anti-Static: Junk Code (мусорные инструкции), рандомизация меток, шифрование строк через XOR.
## 2. Kill-Switch: Программа затирает полностью себя после того как попытаются разобрать бинарник, в RAM, на диске, делая реверс-инжиниринг очень трудным
## 3. FS (Final Stage): В будущих итерациях — сборка кода в памяти с полным удалением следов.

Примечание: Для отладки самого компилятора используйте интерпретатор smapp.py или флаг --debug (в разработке).

---

**C (Clang -O3 + LTO)**

* CRC: 0.38s
* Sum: 0.72s
* Hash: 0.06s
* Sieve: 0.09s
* Matmul: 0.01s

**SMA++ (MLS)**

* CRC: 0.32s 
* Sum: 0.71s 
* Hash: 0.06s 
* Sieve: 0.43s 
* Matmul: 0.07s 

**В чем SMA++ уступает Си:**
SMA++ отстает в задачах **Sieve** и **Matmul**, потому что в MLS нет SIMD штуки.


## 🗺 Дорожная карта (Roadmap)

☑ Парсер, IR, валидаторы, интерпретатор.
☑ 13 классических бэкендов (C, Rust, Go и др.).
☑ Собственный бэкенд MLS (x86-64, syscalls).
☑ Шифрование строк и базовый Anti-Debug.
☐ Починка циклов в MLS (в процессе).
☐ Полноценные библиотеки SMA++ (math, io, string).
☐ Написание драйвера Governor на SMA++.

---

## ⚠️ Статус

## SMA++ находится в активной разработке. 
GMA — это закрытый исходный код (Closed Source). Использование в коммерческих целях требует лицензии.


## ГЛАВНОЕ, SMA++ выпустит только бинарь GMA когда он будет готов, сейчас просто описание но я делал тесты и там реально скорость уровня ассемблер, и то что тут ни чего нету не означает что проект сдох.

---

<Пиши идею. GMA сделает реализацию, минимум букв, максимум конфиденциальности, автоматика вместо ручного управления, невероятно высокая скорость>