# 🚀 Deploy Automatizado de Aplicação FastAPI com ArgoCD e GitHub Actions

## 📋 Pré-requisitos

Antes de começar, certifique-se de que possui os seguintes itens instalados e configurados:

- 🧑‍💻 [Conta no GitHub](https://github.com/signup) (repositórios públicos)  
- 🐳 [Conta no Docker Hub](https://hub.docker.com/signup) com **token de acesso**  
- 🧠 [Rancher Desktop](https://rancherdesktop.io/) com **Kubernetes habilitado**  
  > **Importante:** No painel inicial, vá em `Preferences → Kubernetes` e marque **Enable Kubernetes**  
- ⚙️ `kubectl` configurado corretamente  
- 🚢 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/getting_started/) instalado no cluster local  
- 🧩 [Git](https://git-scm.com/downloads) instalado  
- 🐍 [Python 3](https://www.python.org/downloads/) e [Docker](https://www.docker.com/products/docker-desktop/) instalados  

---

## 🧱 1. Criando repositórios no GitHub

Crie **dois repositórios públicos** no GitHub:

- `hello-app`
- `hello-manifest`

Para isso, entre na sua conta do GitHub e procure por Repositories → New:

<img width="1228" height="111" alt="image" src="https://github.com/user-attachments/assets/0c85a8a7-1352-4571-b4ba-7922599b7c1c" />

Exemplo de saída esperada:

<img width="811" height="215" alt="image" src="https://github.com/user-attachments/assets/0f91020a-2e38-4a79-8b30-c045a756e462" />

---

## 📁 2. Estruturando o projeto localmente

No terminal (Powershell):

Crie uma pasta chamada ‘testeaqui’, ou com qualquer outro nome, para melhor organização:

```bash
mkdir testeaqui
cd testeaqui
```

Clone os repositórios:

```bash
git clone <url-repositorio-hello-app>
git clone <url-repositorio-hello-manifest>
```

Verifique com o comando :
```bash
ls
```

Saída esperada:

<img width="608" height="200" alt="image" src="https://github.com/user-attachments/assets/4d639424-3149-4fbc-b0a7-c19d02e58e06" />


Abra no VS Code:

```bash
code .
```

No VS Code, a estrutura esperada será:

```
testeaqui/
├── hello-app/
└── hello-manifest/
```

Saída esperada:

<img width="222" height="142" alt="image" src="https://github.com/user-attachments/assets/7d44f1c7-7147-4360-9313-445b6e5af311" />

---

Observações:

Durante o projeto, você é possível seguir instruções de criar e editar arquivos e pastas pelo VS Code ou pelo terminal.

No Vs Code, você pode navegar conforme a imagem:

  1 - Cria arquivo
  2 - Cria pasta
  
<img width="282" height="64" alt="image" src="https://github.com/user-attachments/assets/124d1957-8882-4afc-8c8c-c5a9cf284999" />

No terminal, esses comandos podem ser úteis:

```bash
cd <caminho>     # entra em uma pasta
cd ..            # volta uma pasta
pwd              # mostra onde você está
mkdir NomeDaPasta   # cria uma nova pasta
New-Item "nome_arquivo.txt"   # cria um novo arquivo
code .           #abre o Vs Code no diretório
```

## ⚙️ Estrutura do repositório `hello-app`

Crie os seguintes arquivos dentro da pasta hello-app (no Vs Code ou no terminal):

### 🐳 `Dockerfile`
```Dockerfile
# código aqui
```

### 🐍 `main.py`
```python
# código aqui
```

### 📦 `requirements.txt`
```txt
# código aqui
```

**Os três arquivos:**
- `main.py`: código principal da aplicação FastAPI  
- `Dockerfile`: instruções para criar a imagem Docker da aplicação  
- `requirements.txt`: dependências Python necessárias para o app funcionar

Saída esperada no Vs Code:

<img width="246" height="91" alt="image" src="https://github.com/user-attachments/assets/bb4c48f5-2ed5-4b47-ab2a-1a8ac66963e6" />

Saída esperada no terminal:

<img width="679" height="249" alt="image" src="https://github.com/user-attachments/assets/bebae959-a58d-4d04-9c91-b58c2d5156de" />


Voltando ao terminal, na pasta hello-app, não se esqueça de adicionar o novo conteúdo ao repositório do GitHub com:

```bash
git add .
git commit -m “sua mensagme de commit”
git push
```
---

## ⚙️ Estrutura do repositório `hello-manifest`

Na pasta `hello-manifest`, crie a estrutura:

```
hello-manifest/
└── hello-app/
    ├── service.yaml
    └── deployment.yaml
```

Saída esperada:

<img width="288" height="74" alt="image" src="https://github.com/user-attachments/assets/5da02613-cdaf-4ceb-8d9f-4fcdc3d1234a" />


### `service.yaml`
```yaml
# código aqui
```

### `deployment.yaml`
```yaml
# código aqui
```

**Explicação:**
- `deployment.yaml`: define como a aplicação será executada no Kubernetes  
- `service.yaml`: expõe a aplicação  


---

## ☁️ 3. Criando chaves SSH e adicionando segredos no GitHub

### 🔑 Gerar chave SSH

No terminal:

```bash
ssh-keygen -t rsa -b 4096 -C "github-actions@fastapi" -f $env:USERPROFILE\.ssh\projeto_app
```

Pressione **Enter** duas vezes.  


Depois, rode o comando para exibir a chave pública e guarde sua saída:

```bash
cat ~/.ssh/projeto_app.pub
```
Saída esperada:

<img width="1095" height="134" alt="image" src="https://github.com/user-attachments/assets/2f45eb39-7014-441b-9867-1f5dbb208140" />

---

### 🔧 Adicionar chave no GitHub (Deploy Key)

No repositório **`hello-manifest`**:

1. Vá em **Settings → Deploy keys → Add deploy key**

<img width="778" height="128" alt="image" src="https://github.com/user-attachments/assets/9adbc10d-b451-4e9d-96ba-8978b3286d47" />

<img width="1103" height="605" alt="image" src="https://github.com/user-attachments/assets/655ed132-0de3-4c93-819f-d2da1f82d202" />


2. Preencha:
   - **Title:** `app-key`
   - **Key:** cole o conteúdo da saída do último comando
   - **Marque:** “Allow write access”
3. Clique em **Add key**


<img width="722" height="417" alt="image" src="https://github.com/user-attachments/assets/e99e19d3-09c9-4a82-940b-ec2a3b8708b7" />


---

### 🧷 Adicionar chave privada como Secret

Exiba a chave privada:

```bash
cat ~/.ssh/hello_app_ci
```

Saída esperada:

<img width="667" height="312" alt="image" src="https://github.com/user-attachments/assets/897287fb-227c-4fd9-94a4-caecb39d95dd" />


No repositório **`hello-app`**:

1. Vá em **Settings → Secrets and variables → Actions**
2. Clique em **New repository secret**

<img width="1061" height="618" alt="image" src="https://github.com/user-attachments/assets/0000aff5-9bd4-409c-a352-a9e8856835d1" />

A tela seguinte é semelhante à tela de preenchimento da chave anterior. Nela, preencha:

3. Nome: `SSH_PRIVATE_KEY`
4. Valor: cole a saída do último comando
5. Clique em **Add secret**

---

### 🐳 Adicionar Secrets do Docker Hub

1. Gere um **Personal Access Token** no [Docker Hub → ícone de Perfil  → Account Settings → Personal Access Tokens](https://hub.docker.com/settings/security)

<img width="1320" height="624" alt="image" src="https://github.com/user-attachments/assets/ae20b1dd-daf2-41fa-86b1-bf59fcb821d7" />

   - Clique em “Generate New Token”

<img width="1343" height="583" alt="image" src="https://github.com/user-attachments/assets/10570e23-2d26-4364-81d1-3fd247b0fa66" />

   - Descrição: `github-actions`
   - Permissão: `Read, Write, Delete`
   - Copie o token gerado

<img width="639" height="397" alt="image" src="https://github.com/user-attachments/assets/8c7d2ccb-66b0-4847-8318-7c2f7495768a" />


Obs: o usuário do Docker hub pode ser visto na tela incial dos repositórios, quando se clica no ícone do perfil:

<img width="336" height="396" alt="image" src="https://github.com/user-attachments/assets/d40cc922-de5f-47f9-bcdd-6075a3b822c7" />



2. No repositório **`hello-app`**, no mesmo caminho do último secret, adicione:
   - `DOCKER_USERNAME`, com valor sendo seu usuário Docker Hub  
   - `DOCKER_PASSWORD`, com valor sendo seu token gerado
  
Saída esperada das secrets do repositório:

<img width="722" height="244" alt="image" src="https://github.com/user-attachments/assets/94e90b2a-4d7d-4a94-9919-6475431ae055" />

---

## ⚙️ 4. Criando o GitHub Actions (CI/CD)

No repositório **`hello-manifest`**, crie a seguinte estrutura:
1. uma pasta '.github'
2. Dentro dela, uma pasta 'workflows'
3. Dentro dela, um arquivo chamando 'ci-cd.yaml'

```
.github/
└── workflows/
    └── ci-cd.yaml
```

Saída esperada no VsCode:

<img width="286" height="133" alt="image" src="https://github.com/user-attachments/assets/8adcb680-d721-4afa-a61d-b511213c85a0" />


Arquivo **`ci-cd.yaml`**:
```yaml
# código aqui
```

No terminal, na pasta hello-manifest, não se esqueça de dar commit:

```bash
git add .
git commit -m "Adiciona pipeline CI/CD"
git push
```

---

## 🧪 6. Testando a automação

1. Edite o arquivo `main.py` (no VSCode ou GitHub)  
2. Faça commit e push  
3. Verifique:
   - ✅ A imagem foi atualizada no **Docker Hub**
   - ✅ Um **Pull Request** foi criado no repositório `hello-manifest`
   - ✅ O arquivo `deployment.yaml` foi atualizado com a nova tag da imagem  

colocar imagens do teste aqui lembrar
---

## 🧭 7. Acessando o ArgoCD

### Instalar ArgoCD
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd -w
```

Verifique se está ativo:
```bash
kubectl get pods -n argocd
```

<img width="826" height="171" alt="image" src="https://github.com/user-attachments/assets/36c5d58d-6366-4d3a-8f3c-7b49f67379ab" />

Configure a porta de acesso:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Abra no navegador:  
👉 [https://localhost:8080](https://localhost:8080)

Recupere a senha de acesso:
```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
```

Decodifique a senha:
```powershell
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("SAÍDA_DO_COMANDO_ACIMA"))
```

Login:
- **User:** `admin`  
- **Senha:** obtida no comando acima

tela de aceso aqui imagem

---

## 🚀 8. Criar App no ArgoCD

No painel do ArgoCD:

1. Clique em **New App**

<img width="1024" height="133" alt="image" src="https://github.com/user-attachments/assets/60b11e64-40e5-4e99-b023-92715d395728" />

2. Configure:
   - **Application name:** `hello-app`
   - **Project:** `default`
   - **Sync policy:** automática (como no último projeto)
   - **Repository URL:** seu repositório `hello-manifest`
   - **Revision:** `main`
   - **Path:** `hello-app`
   - **Cluster URL:** `in-cluster`
   - **Namespace:** `default`

3. A aplicação deve aparecer como **Healthy** ✅  

<img width="425" height="431" alt="healthy" src="https://github.com/user-attachments/assets/e9308666-84fd-4346-8766-42455e4c37db" />

Acesse no navegador para ver:  
👉 [http://localhost:30080/](http://localhost:30080/)

---

## 🔁 Teste final

1. Altere a mensagem no `main.py`
2. Aceite o **Pull Request** no repositório `hello-manifest`
3. Verifique no ArgoCD:
   - O app ficará **Out of Sync**

<img width="424" height="367" alt="outofsyncteste" src="https://github.com/user-attachments/assets/0c368c6e-e310-4907-81e0-9e882ef78f9d" />


   - Depois sincronizará automaticamente


<img width="424" height="367" alt="outofsyncteste" src="https://github.com/user-attachments/assets/bbc05f12-4c65-4ec3-9ea5-06035c2a466d" />


   - A nova mensagem aparecerá em `http://localhost:30080/`

---
