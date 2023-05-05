# helm-backend
Helm chart for backends applications

# Como atualizar chart
```bash
git clone helm-backend
cd helm-backend
#Antes de gerar o pacote atualizar a versao do chart no arquivo chart.yaml
helm package ~/folder #Pasta com o template e arquivo chart.yaml e gera o *.tgz no diretorio atual
helm repo index . #Atualiza o index.yaml com a versao
git add .
git commit -m "update repo"
git push
```
