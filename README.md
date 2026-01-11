# First_Azure-Function_Java
Azure Functions | Http Trigger Template | Maven | Local Azure Functions | Java 21 

---

## Passo a passo para que você possa criar a sua função baseada em gatilho HTTP  

1. Tenha o Maven e Java instalado (Use uma versão compátivel com as versões disponíveis no Azure function)

  > Instalação para linux ubuntu/debian:

    apt install maven openjdk-21-jdk
  > Se não tiver permissões use `sudo` como sufixo do comando instruído

2. Com maven crie um projeto com arquetipo específico para funções azure  

       mvn archetype:generate "-DarchetypeGroupId=com.microsoft.azure" "-DarchetypeArtifactId=azure-functions-archetype" "-DjavaVersion=21"

    > Nesse caso usando a versão 21 do java
- Isso criará seu projeto (você ainda informará seu groupId e artifactId) usando o template da microsoft azure estruturado para Azure functions.

3. Crie suas funções  - declaradas pela anotação @FunctionName("NomeDaFunção") - e defina os métodos de associação

4. Para teste local da função: Instale as Ferramentas da Azure Functions
> Instalações disponíveis em:  <a src="https://github.com/Azure/azure-functions-core-tools" alt="Link para Github da azure repo da ferramenta para Azure functions">https://github.com/Azure/azure-functions-core-tools</a>  
> Sem isso o build irá dar erro

5. Build o projeto:
    
       mvn clean package
      > Se desejar pular os testes adicione o argumento `-DskipTests`

6. Teste localmente:

       mvn azure-functions:run
      > Só funcionará se tiver a ferramenta instalada para uso do plugin

Logs local:  
<img width="1894" height="951" alt="Capturadetela" src="https://github.com/user-attachments/assets/f1240d9c-9737-47e6-89be-503e2929dfde" />
Um response - associção do trigger de Get no endpoint `api/NumeroAleatorio`:  
<img width="515" height="123" alt="Capturadetela" src="https://github.com/user-attachments/assets/4fd222d6-fea9-4ff3-b756-feddbff8d6b4" />

#### Funcionou? Muito bem!!👏 
#### Não funcionou. Algum passo da instrução deu errado? Abre uma issue aí explicando o que aconteceu 📑
> Instruções dadas considerando que você esteja no diretório do projeto (onde está localizado o pom.xml)
> Atentem-se a configurar os path diretinho

## Quer deployar a Função Azure? Vem comigo

Temos algumas formas de fazer o deploy. Primeiro, o "executável" das funções que você criou ficam no dir target/, mas específicamente na pasta `/azure-functions/<ArtifactId-Que-Você-Definiu-$TIMESTAMP>/`.   
Nessa pasta as funções ficam como subpastas e possuem seu arquivo de configuração de função. Além de termos a pasta `lib/`, arquivos `host.json` e `local.settings.json` e o `.jar` das funções. Com exceção do `local.settings.json`, são as "configurações" que precisamos para implantar nossa função. 

> Sua funções precisam de um App Function onde devem ser implantadas. E as funções necessitam de storage account para funcionar, pois tratam host, triggers, bindings e logs internos.

1. A azure oferece diferentes maneiras de implantar as funções, vou mostrar a que usa arquivos zipados pelo Portal Azure e CLI Azure:

      1. No seu App function no portal da azure, nas seções à esquerda acesse Deployment -> Deploymente Center: clique na seção ao lado direito de source e escolha __Publish files__ e então selecione a pasta zipada com as configurações da sua função 
          <img width="1464" height="817" alt="Capturadetela" src="https://github.com/user-attachments/assets/5f906662-2ba0-415b-85f5-d05988ea52f3" />

      2. Ou ainda pelo CLI:

              az functionapp deployment source config.zip -g <resource-group> -n <nome-app-function> --src <caminho/do/seu/arquivo/zipado>.zip
         ou diretamente do seu projeto:

             mvn azure-functions:deploy
           > Ele se baseia nas informações declaradas no pom.xml ou até em argumentos passados 

2. Ou ainda, utilizando o Github com Actions, onde no lugar do __Publish files__ você seleciona __Github__, loga na sua conta, escolhe o repositório e define se deseja criar um workflow manual ou se deixará com que seja criado automaticamente.

---

Optei por ativar os insigths e pude acompanhar logs e métricas pelo painel do Application insights que apresenta os dados do Logs Analytis Workspace de maneira organizada e permitindo monitoramento contínuo da execução.

<img width="1880" height="760" alt="Capturadetela" src="https://github.com/user-attachments/assets/84915586-d592-4128-a502-df2fcfa2366d" />

<img width="1799" height="794" alt="Capturadetela" src="https://github.com/user-attachments/assets/4d030b97-4319-4dad-ba9b-52cd18a53ac7" />
