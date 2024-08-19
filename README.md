# Documentação da Atividade - Configuração AWS

## Requisitos AWS

### 1. Gerar uma Chave Pública para Acesso ao Ambiente

1. No Console de Gerenciamento da AWS, vá para **EC2**.
2. No menu lateral, clique em **Key Pairs**.
3. Clique em **Create key pair**.
4. Nomeie a chave.
5. Selecione o formato **.pem** e clique em **Create key pair**.
6. Baixe o arquivo `.pem` e armazene-o em um local seguro.

### 2. Criar uma Instância EC2

1. No Console de Gerenciamento da AWS, vá para **EC2**.
2. Clique em **Launch Instance**.
3. Escolha a **Amazon Linux 2 AMI** (Amazon Machine Image).
4. Selecione a instância **t3.small**.
5. Clique em **Next: Configure Instance Details** e configure conforme necessário.
6. Clique em **Next: Add Storage** e configure o armazenamento como **16 GB SSD**.
7. Clique em **Next: Add Tags** e adicione tags conforme necessário.
8. Clique em **Next: Configure Security Group**.
9. Selecione **Create a new security group**.
10. Adicione as seguintes regras para as portas de comunicação:
   - **Type**: SSH, **Protocol**: TCP, **Port Range**: 22, **Source**: My IP
   - **Type**: NFS, **Protocol**: TCP, **Port Range**: 111, **Source**: Anywhere
   - **Type**: NFS, **Protocol**: UDP, **Port Range**: 111, **Source**: Anywhere
   - **Type**: NFS, **Protocol**: TCP, **Port Range**: 2049, **Source**: Anywhere
   - **Type**: NFS, **Protocol**: UDP, **Port Range**: 2049, **Source**: Anywhere
   - **Type**: HTTP, **Protocol**: TCP, **Port Range**: 80, **Source**: Anywhere
   - **Type**: HTTPS, **Protocol**: TCP, **Port Range**: 443, **Source**: Anywhere
11. Clique em **Review and Launch**.
12. Revise as configurações e clique em **Launch**.
13. Selecione a chave pública criada anteriormente e clique em **Launch Instances**.
14. Acesse a instância após a inicialização.

### 3. Gerar um Elastic IP e Anexar à Instância EC2

1. No Console de Gerenciamento da AWS, vá para **EC2**.
2. No menu lateral, clique em **Elastic IPs**.
3. Clique em **Allocate Elastic IP address** e depois em **Allocate**.
4. Selecione o Elastic IP recém-criado e clique em **Actions** > **Associate Elastic IP address**.
5. Escolha a instância EC2 para associar o Elastic IP e clique em **Associate**.

## Requisitos no Linux

### 1. Configurar o NFS Entregue

1. **Instalar o NFS**:
   ```bash
   sudo apt-get update
   sudo apt-get install nfs-kernel-server

2. **Criar o diretório a ser compartilhado**:
   ```bash
   sudo mkdir -p /mnt/nfs_share

3. **Configurar o arquivo de exportação**:
   ```bash
   sudo nano /etc/exports
3.1 Adicione a linha:
   ```bash
/mnt/nfs_share *(rw,sync,no_subtree_check)

   
