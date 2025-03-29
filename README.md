<div align="center" style="font-family: Arial, sans-serif; font-size: 20px; line-height: 1.5;">

# 🌟 **Curso AWS Developer-EDN** 🌟
# 🌟 Developer Associate  🌟

###
<a href="https://escoladanuvem.org"><a href="https://aws.amazon.com/pt/?nc2=h_lg">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/2560px-Amazon_Web_Services_Logo.svg.png" width="150" alt="AWS Logo">
</a>

<img src="https://files.svgcdn.io/logos/aws-ec2.svg" width="80" alt="AWS Logo">-
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQV1xf24ZfPXmIIRZRYnCyvZVqn4mSD1b79xw&s" width="80" alt="AWS Logo"/>-
    <img src="https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/download%20(4)_processed.png?raw=true" width="180" alt="Second Image">
</a>

</div>

# Laboratório 1: Explorando a AWS com o Amazon EC2
**Turma:** DEV 02  
**Aluno:** Guilherme Barros 

## Objetivos:
- Iniciar um servidor web de teste via **console da AWS**;
- Configurar o servidor para permitir **conexões HTTP**;
- Iniciar uma instância **EC2 via CloudShell**.

## Introdução:
Este laboratório fornece uma visão básica e prática sobre instâncias EC2, abordando como inicializá-las pelo console da AWS e via CloudShell. O Amazon Elastic Compute Cloud (EC2) é um serviço que oferece poder computacional por meio de máquinas virtuais (VMs), permitindo que desenvolvedores provisionem recursos computacionais de maneira rápida e eficiente.

## Pré-requisitos:
- **Conta AWS** configurada previamente pela Escola da Nuvem.

---

## Passos do Laboratório

### 1. Configurar e Iniciar uma Instância EC2 pelo Console AWS
1. Acesse o console da AWS com a conta fornecida.
2. No canto superior esquerdo, pesquise por "EC2" e acesse o serviço.
3. Clique em "Executar Instância".
4. Configure os seguintes parâmetros:
   - **Nome**: webserver-`seu-nome-sobrenome` (sem espaços ou caracteres especiais)
   - **AMI**: Amazon Linux 2 AMI (HVM)
   - **Tipo de instância**: t2.micro
   - **Par de chaves**: Criar novo par de chaves com nome `parchave-seunome`
   - **Tipo de par de chaves**: RSA
   - **Formato**: .pem (salve o arquivo com segurança)
   - **Grupo de segurança**: Criar um novo e permitir tráfego HTTP
   - **Dados de usuário**:
     ```bash
     #!/bin/bash
     yum -y install httpd
     systemctl enable httpd
     systemctl start httpd
     echo '<html><h1>Olá do seu servidor web!</h1></html>' > /var/www/html/index.html
     ```
5. Clique em "Executar Instância".
6. Aguarde a verificação de status 2/2.
7. Copie o **Endereço IPv4 Público** da instância.
8. No navegador, acesse `http://seu-ip-publico` e verifique se o servidor web está funcionando.

---

### 2. Inicializar Instância EC2 via CloudShell
1. No console da AWS, clique no ícone do **CloudShell** no canto superior direito.
2. Execute os seguintes comandos (substitua `seunome` pelos seus dados):

   ```bash
   GRUPO_SEGURANCA="seunome-grupo"
   NOME_INSTANCIA="instancia-seunome"
   PAR_CHAVE="parchave-seunome"
   ```

3. Criar grupo de segurança e permitir tráfego HTTP:
   ```bash
   SECURITY_GROUP_ID=$(aws ec2 create-security-group --group-name $GRUPO_SEGURANCA --description "Permitir HTTP" --query "GroupId" --output text)
   aws ec2 authorize-security-group-ingress --group-id $SECURITY_GROUP_ID --protocol tcp --port 80 --cidr 0.0.0.0/0
   ```

4. Criar a instância EC2:
   ```bash
   aws ec2 run-instances --instance-type t2.micro --image-id $(aws ssm get-parameters-by-path --path "/aws/service/ami-amazon-linux-latest" --query "Parameters[?ends_with(Name, 'al2023-ami-kernel-default-x86_64')].Value" --output text) --security-group-ids $SECURITY_GROUP_ID --tag-specifications "ResourceType=instance,Tags=[{Key=Name,Value='$NOME_INSTANCIA'}]" --key-name $PAR_CHAVE --user-data "IyEvYmluL2Jhc2gKeXVtIC15IGluc3RhbGwgaHR0cGQKc3lzdGVtY3RsIGVuYWJsZSBodHRwZApzeXN0ZW1jdGwgc3RhcnQgaHR0cGQKZWNobyAnPGh0bWw+PGgxPk9sw6EgZG8gc2V1IHNlcnZpZG9yIHdlYiE8L2gxPjwvaHRtbD4nID4gL3Zhci93d3cvaHRtbC9pbmRleC5odG1sCg=="
   ```
---

### 3. Finalizar as Instâncias
1. No console da AWS, acesse "EC2 > Instâncias".
2. Selecione as instâncias criadas e clique em **Encerrar instância**.
3. Acesse "EC2 > Grupos de Segurança", localize o grupo criado e exclua-o.
4. Clique no nome de usuário (canto superior direito) e selecione **Sair**.

---

## Recursos Adicionais
- [Iniciar uma instância Amazon EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
- [Tipos de Instâncias EC2](https://aws.amazon.com/pt/ec2/instance-types/)
- [Amazon Machine Images (AMI)](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/AMIs.html)
- [Grupos de Segurança no EC2](https://docs.aws.amazon.com/pt_br/AWSEC2/latest/UserGuide/ec2-security-groups.html)

---

## Demonstração em Vídeo
### Configuração da EC2 via Console AWS
https://github.com/user-attachments/assets/23ab995a-2273-4d18-b13b-241a595839c4

### Configuração da EC2 via CloudShell
https://github.com/user-attachments/assets/191d1c11-adc4-4b1b-b016-6c18a6f48140

---

**Laboratório concluído com sucesso!** 🎉

