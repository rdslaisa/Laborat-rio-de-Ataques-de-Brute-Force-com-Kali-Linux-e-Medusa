# Laboratório de Ataques de Brute Force com Kali-Linux e Medusa

## Laboratório prático realizado em ambiente controlado de segurança ofensiva utilizando Kali-Linux, Medusa, DVWA e Metasploitable 2 para simulação de ataques de  brute force e password spraying.

---
### :mag: Reconhecimento e Enumeração

Nesta etapa, foi realizado o processo de reconhecimento e exploração da máquina alvo utilizando o Nmap no ambiente controlado do laboratório.

:microscope:O objetivo foi identificar:

portas abertas;
serviços ativos;
versões dos serviços;
possíveis vetores de ataque.

<img width="1920" height="974" alt="ftp_process" src="https://github.com/user-attachments/assets/eb8ca517-5c5d-47b9-bc8c-ad1239064a17" />

Foi contatado um serviço potencialmente vulnerável executando na porta 21/TCP (FTP). Utilizando Wordlists de usuários e senhas clássicas e a ferramenta Medusa, foi possível saber o usuário e senha corretos. Sendo eles escritos da mesma forma: *msfadmin*

<img width="1920" height="974" alt="ftp_sucess" src="https://github.com/user-attachments/assets/50507413-53c5-467d-8664-44303297a1d0" />

:dart:Possiveis medidas de mitigação:
substituir FTP por SFTP;
utilizar senhas fortes;
habilitar MFA;
limitar tentativas de login;
restringir acesso por firewall.

---
### :globe_with_meridians: Automação de Login Web — DVWA

Após a enumeração dos serviços, foi identificado o acesso à aplicação vulnerável DVWA através da porta HTTP.

URL acessada: http://192.168.56.101/dvwa/login.php.

:computer:Foi realizado uma tentativa inválida para a inspeção do formulário de autenticação, utilizando as ferramentas de desenvolvedor do navegador foi possível identificar:

método de envio;
parâmetros POST;
campos do formulário;
comportamento da autenticação.

Parâmetros identificados:

**username**   
**password**  
**Login**

<img width="1920" height="974" alt="dvwan error" src="https://github.com/user-attachments/assets/a0d07b16-38a8-4192-8238-e6fa3cc3c354" />

Durante os primeiros testes com o Medusa, ocorreram mensagens de erro relacionadas à configuração incorreta dos parâmetros HTTP do formulário.

Isso demonstrou a importância da identificação correta:

da estrutura do POST;
dos parâmetros esperados;
da mensagem de falha de autenticação.

 Após os ajustes necessários, o ataque automatizado foi executado com sucesso utilizando Wordlists simples de usuários e senhas. O Medusa identificou múltiplas combinações válidas de credenciais, permitindo autenticação na aplicação DVWA. Exemplos encontrados:

**user : password**  
**admin : password**  
**msfadmin : msfadmin**

<img width="1920" height="974" alt="dvwa sucess" src="https://github.com/user-attachments/assets/ab7414c3-d685-4d19-8798-e9c290fb5bc9" />

Podendo assim ter acesso dentro da página:

<img width="1920" height="974" alt="dentro dvwa" src="https://github.com/user-attachments/assets/86d8a308-6a31-4f65-a3fb-1f635031bb1f" />

 O teste demonstrou como credenciais fracas, ausência de bloqueio de tentativas, aplicações inseguras e autenticação sem proteção podem facilitar ataques automatizados de brute force.
 
:dart:Recomendações de Mitigação:
utilização de senhas fortes;
autenticação multifator (MFA);
limitação de tentativas de login;
CAPTCHA;
implementação de rate limiting.

---

### :unlock: Password Spraying em SMB

  Na fase final do labolatório, a primeira etapa consistiu em identificar informações sobre o serviço SMB no alvo através da enumeração.

<img width="1920" height="974" alt="acessando o enum4" src="https://github.com/user-attachments/assets/936a279f-f558-45aa-91f8-dd0a218aad65" />  

 Com base na enumeração, foram criadas Wordlists para realizar o teste de credenciais e em conjunto o Medusa para testar as combinações de usuários e senhas contra o serviço SMB. Resultando o sucesso na autenticação do usuário *msfadmin* com a senha *msfadmin*.

<img width="1920" height="974" alt="smb-_sucess" src="https://github.com/user-attachments/assets/95a2bcd4-1253-48ee-b3de-6cc54baa0fbe" />

Após obter as credenciais válidas, utilizei o comando **smbclient** para listar os diretórios compartilhados no servidor

<img width="1920" height="974" alt="seção correta" src="https://github.com/user-attachments/assets/0f0a0f38-ee5f-454f-8bf5-d114fa2b6b77" />

O output revelou compartilhamentos críticos como ADMIN$ e IPC$, confirmando o acesso com privilégios administrativos. O teste demonstrou a vulnerabilidade do serviço SMB em configurações padrão (default) e o perigo de utilizar credenciais fracas.

:dart:Recomendação para mitigar riscos: Desabilitar sessões nulas (NULL Sessions); Implementar políticas de senhas fortes.

---
### :memo:Aprendizados Técnicos

Este laboratório permitiu compreender como serviços inseguros e credenciais fracas representam riscos reais em ambientes corporativos. A utilização do Medusa demonstrou a eficiência de ataques automatizados contra autenticação vulnerável, reforçando a importância de políticas robustas de segurança, monitoramento contínuo e autenticação multifator.

---
### :warning:Alerta

Este projeto foi desenvolvido exclusivamente para fins de pesquisa em segurança da informação, utilizando ambientes controlados e autorizados, como Kali-Linux, Metasploitable 2 e DVWA.

Todos os testes, simulações de brute force, enumeração e autenticação automatizada foram realizados em máquinas virtuais locais, sem qualquer interação com sistemas públicos, terceiros ou ambientes não autorizados.

O objetivo deste laboratório é demonstrar:

conceitos de segurança ofensiva;
riscos de credenciais fracas;
funcionamento de ataques automatizados;
e boas práticas de mitigação e defesa.

O uso inadequado das técnicas, ferramentas ou conhecimentos apresentados neste repositório contra sistemas sem autorização é antiético e pode violar leis e regulamentações de segurança digital.


