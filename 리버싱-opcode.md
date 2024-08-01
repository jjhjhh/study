🔗https://learn.dreamhack.io/571#2 드림핵 강의를 참고하였습니다 

<br>

push val : val을 스택 최상단에 쌓음 

```nasm
rsp -= 8
[rsp] = val
```

<br>

pop reg : 스택 최상단의 값을 꺼내서 reg에 넣음 

```nasm
rsp += 8
reg = [rsp-8] //먼저 rsp를 넣고 rsp+=8하는 것이 아님 
```

<br>

call addr : addr에 위치한 프로시져 호출 (함수호출)

```nasm
push return_address //리턴주소(call 다음의 명령어 주소)를 스택에 쌓고 이동 
jmp addr
```

<br>

leave : 스택프레임 정리

```nasm
mov rsp, rbp //rsp가 rbp와 같은 공간을 가리키도록 함 
pop rbp //스택 최상단 **값**을 넣음. rsp+=8 됨 
```

```nasm
실행 전 

[Register]
rsp = 0x7fffffffc400
**rbp = 0x7fffffffc480**

[Stack]
0x7fffffffc400 | 0x0 <- rsp
...
0x7fffffffc480 | 0x7fffffffc500 <- rbp
0x7fffffffc488 | 0x31337 

[Code]
leave
```

```nasm
실행 후 

[Register]
rsp = 0x7fffffffc488
rbp = 0x7fffffffc500

[Stack]
0x7fffffffc400 | 0x0
...
0x7fffffffc480 | 0x7fffffffc500
0x7fffffffc488 | 0x31337 <- rsp
...
0x7fffffffc500 | 0x7fffffffc550 <- rbp
```

일 때, mov rsp, rbp에 의해 rsp=0x7fffffffc480 가 됨

이후 pop rbp에 의해 스택 최상단 (rsp=0x7fffffffc480)의 값을 갖게 됨. `rbp=0x7fffffffc500`

그리고 rsp+=8 으로 `rsp=0x7fffffffc488`을 가리키게 된다.

<br>

ret : return address로 반환

```nasm
pop rip
```

pop ⇒ rsp+=8  ; mov rip, [rsp-8]
