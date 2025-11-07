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

Exemplo de saída esperada:

<img width="811" height="215" alt="image" src="https://github.com/user-attachments/assets/0f91020a-2e38-4a79-8b30-c045a756e462" />

---

## 📁 2. Estruturando o projeto localmente

No terminal (Powershell):

Crie uma pasta chamada ‘testeaqui’, ou com qualquer outro nome, para melhor organização

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
aquiprojeto/
├── hello-app/
└── hello-manifest/
```

Saída esperada:

<img width="222" height="142" alt="image" src="https://github.com/user-attachments/assets/7d44f1c7-7147-4360-9313-445b6e5af311" />

---

## ⚙️ Estrutura do repositório `hello-app`

Crie os seguintes arquivos:

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

**Explicação:**
- `main.py`: código principal da aplicação FastAPI  
- `Dockerfile`: instruções para criar a imagem Docker da aplicação  
- `requirements.txt`: dependências Python necessárias para o app  

---

## ⚙️ Estrutura do repositório `hello-manifest`

Na pasta `hello-manifest`, crie a estrutura:

```
hello-manifest/
└── hello-app/
    ├── service.yaml
    └── deployment.yaml
```

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
- `service.yaml`: expõe a aplicação dentro (ou fora) do cluster  

---

## ☁️ 3. Criando chaves SSH e adicionando segredos no GitHub

### 🔑 Gerar chave SSH

No terminal:

```bash
ssh-keygen -t rsa -b 4096 -C "ci@hello-app" -f $env:USERPROFILE\.ssh\hello_app_ci
```

Pressione **Enter** duas vezes.  
Depois, exiba a chave pública:

```bash
cat ~/.ssh/hello_app_ci.pub
```

---

### 🔧 Adicionar chave no GitHub (Deploy Key)

No repositório **`hello-manifest`**:

1. Vá em **Settings → Deploy keys → Add deploy key**
2. Preencha:
   - **Title:** `ci-deploy-key`
   - **Key:** cole o conteúdo do arquivo `.pub`
   - **Marque:** “Allow write access”
3. Clique em **Add key**

---

### 🧷 Adicionar chave privada como Secret

Exiba a chave privada:

```bash
cat ~/.ssh/hello_app_ci
```

No repositório **`hello-app`**:

1. Vá em **Settings → Secrets and variables → Actions**
2. Clique em **New repository secret**
3. Nome: `SSH_PRIVATE_KEY`
4. Valor: cole a chave privada
5. Clique em **Add secret**

---

### 🐳 Adicionar Secrets do Docker Hub

1. Gere um **Personal Access Token** no [Docker Hub → Account Settings → Personal Access Tokens](https://hub.docker.com/settings/security)  
   - Clique em “Generate New Token”  
   - Descrição: `github-actions`
   - Permissão: `Read, Write, Delete`
   - Copie o token gerado  

2. No repositório **`hello-app`**, adicione:
   - `DOCKER_USERNAME` → seu usuário Docker Hub  
   - `DOCKER_PASSWORD` → o token gerado  

---

## ⚙️ 4. Criando o GitHub Actions (CI/CD)

No repositório **`hello-manifest`**, crie a seguinte estrutura:

```
.github/
└── workflows/
    └── ci-cd.yaml
```

Arquivo **`ci-cd.yaml`**:
```yaml
# código aqui
```

Adicione ao repositório:

```bash
cd aquiprojeto/hello-manifest
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

Abra no navegador:  
👉 [https://localhost:8080](https://localhost:8080)

Recupere a senha:
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

---

## 🚀 8. Criar App no ArgoCD

No painel do ArgoCD:

1. Clique em **New App**
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

Acesse no navegador:  
👉 [http://localhost:30080/](http://localhost:30080/)

---

## 🔁 Teste final

1. Altere a mensagem no `main.py`
2. Aceite o **Pull Request** no repositório `hello-manifest`
3. Verifique no ArgoCD:
   - O app ficará **Out of Sync**
   - Depois sincronizará automaticamente
   - A nova mensagem aparecerá em `http://localhost:30080/`

---
