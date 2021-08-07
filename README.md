
<h1>Documentação da API com o Swagger 3.0</h1>


Nesta atividade iremos implementar a Documentação da nossa API com o Swagger. 

Utilize o e-book sobre Swagger (<a href="https://github.com/rafaelq80/swagger_blogpessoal/blob/main/ebook/swagger_documentacao_blog_pessoal.pdf" target="_blank">Clique aqui</a>) e as instruções descritas abaixo como referências para implementar  a Documentação da API.

## Boas Práticas

1. Configure as Dependências no arquivo pom.xml
2. SwaggerConfig
3. Executando o Swagger
4. Definindo o Swagger como página principal da API
5. Gerando o PDF da Documentação


<h2 id="#dep">#Passo 1 - Dependências</h2>

Configurando o pom.xml

Insira a seguinte dependência no projeto:
```xml
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-boot-starter</artifactId>
	<version>3.0.0</version>
</dependency>
```



<h2 id="#swg">#Passo 2 - SwaggerConfig</h2>

Crie uma nova package no seu projeto chamada **configuration** , dentro dela crie uma classe chamada SwaggerConfig e configure segundo o  modelo abaixo:

```java

package  br.org.generation.blogpessoal.configuration;

import  java.util.ArrayList;
import  java.util.List;

import  org.springframework.context.annotation.Bean;
import  org.springframework.context.annotation.Configuration;
import  org.springframework.http.HttpMethod;

import  springfox.documentation.builders.ApiInfoBuilder;
import  springfox.documentation.builders.PathSelectors;
import  springfox.documentation.builders.RequestHandlerSelectors;
import  springfox.documentation.builders.ResponseBuilder;
import  springfox.documentation.service.ApiInfo;
import  springfox.documentation.service.Contact;
import  springfox.documentation.service.Response;
import  springfox.documentation.spi.DocumentationType;
import  springfox.documentation.spring.web.plugins.Docket;

@Configuration
public class SwaggerConfig {
	@Bean
	public Docket api() {
		return new Docket(DocumentationType.SWAGGER_2)
		.select()
		.apis(RequestHandlerSelectors
		.basePackage("br.org.generation.blogpessoal.controller"))
		.paths(PathSelectors.any())
		.build()
		.apiInfo(metadata())
		.useDefaultResponseMessages(false)
		.globalResponses(HttpMethod.GET, responseMessage())
		.globalResponses(HttpMethod.POST, responseMessage())
		.globalResponses(HttpMethod.PUT, responseMessage())
		.globalResponses(HttpMethod.DELETE, responseMessage());
	}

	public static ApiInfo metadata() {

		return new ApiInfoBuilder()
			.title("API - Blog Pessoal")
			.description("Projeto API Spring - Blog Pessoal")
			.version("1.0.0")
			.license("Apache License Version 2.0")
			.licenseUrl("https://github.com/rafaelq80")
			.contact(contact())
			.build();
	}

	private static Contact contact() {

		return new Contact("Rafael Queiróz", 
			"https://github.com/rafaelq80", 
			"rafaelproinfo@gmail.com");

	}

	private static List<Response> responseMessage() {

		return new ArrayList<Response>() {

			private static final long serialVersionUID = 1L;

			{
				add(new ResponseBuilder().code("200").description("Sucesso!").build());
				add(new ResponseBuilder().code("201").description("Criado!").build());
				add(new ResponseBuilder().code("400").description("Erro na requisição!").build());
				add(new ResponseBuilder().code("401").description("Não Autorizado!").build());
				add(new ResponseBuilder().code("403").description("Proibido!").build());
				add(new ResponseBuilder().code("404").description("Não Encontrado!").build());
				add(new ResponseBuilder().code("500").description("Erro!").build());
			}
		};

	}
}
```




<h2 id="#exec">#Passo 3 - Executando o Swagger</h2>


Inicie a API, abra o seu navegador na Internet e digite o endereço abaixo para abrir a sua documentação.

<div align="center"><a href="http://localhost:8080/swagger-ui/" /><b>http://localhost:8080/swagger-ui/</b></a></div>

Em aplicações com camadas de segurança, você precisará logar com uma conta de usuário antes de exibir a sua documentação no Swagger. Insira um usuário cadastrado no Banco de Dados local via Postman para fazer o login.

<div align="center"><img src="https://i.imgur.com/LFsXhdV.png" title="source: imgur.com" /></div>

Pronto! A sua documentação no Swagger está funcionando.

<div align="center"><img src="https://i.imgur.com/Ox62rmQ.png" title="source: imgur.com" /></div>



<h2 id="ind">#Passo 4 - Definindo o Swagger como página principal da API</h2>

Vamos configurar o **Swagger** como página principal da nossa API, ou seja, ao digitarmos o endereço: <b>http://localhost:8080</b>, ao invés de abrir a página abaixo:

<div align="center"><img src="https://i.imgur.com/wE5e4hZ.png" title="source: imgur.com" /></div>

Abriremos a página do Swagger.

Na camada principal do nosso projeto Blog Pessoal  (**br.org.generation.blogpessoal**) vamos abrir o arquivo **BlogpessoalApplication**

Vamos alterar o arquivo de:

```java
package br.org.generation.blogpessoal;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BlogpessoalApplication {

	public static void main(String[] args) {
		SpringApplication.run(BlogpessoalApplication.class, args);
	}

}
```

Para:

```java
package br.org.generation.blogpessoal;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.ModelAndView;

@SpringBootApplication
@RestController
@RequestMapping("/")
public class BlogpessoalApplication {

	@GetMapping
	public ModelAndView swaggerUi() {
		
		return new ModelAndView("redirect:/swagger-ui/");
		
	}
	
	public static void main(String[] args) {
		SpringApplication.run(BlogpessoalApplication.class, args);
	}

}
```

As alterações acima transformam a classe principal da nossa API (**BlogpessoalApplication**) em uma classe do tipo **RestController**, que responderá à todas as requisições do tipo **GET** para o **endpoint "/"** (raiz do nosso projeto) e fará o redirecionamento para o Swagger, ou seja, o Swagger será a página home do nosso projeto.



<h2 id="#pdf">#Passo 5 - Gerando o PDF da Documentação</h2>


1) No Swagger, clique no link: <a heref="http://localhost:8080/v2/api-docs" />http://localhost:8080/v2/api-docs para visualizar a documentação no formato JSON.

<div align="center"><img src="https://i.imgur.com/2bHe7sq.png" title="source: imgur.com" /></div>

2) Visualize o código no formato Raw Data (No Chrome e no Edge é o formato padrão), Selecione todo o código (Ctrl + A) e Copie (Ctrl + C).

<div align="center"><img width="850px" src="https://i.imgur.com/vDQyr0H.png" title="source: imgur.com" /></div>

3) Acesse o site **Swagger Editor** (<a heref="https://editor.swagger.io/" />https://editor.swagger.io/).

<div align="center"><img width="900px" src="https://i.imgur.com/ADiczZM.png" title="source: imgur.com" /></div>

4) No <b>Swagger Editor</b>, apague o código exemplo e cole o código copiado da Documentação dentro do Editor (Ctrl + V).

<div align="center"><img width="850px"  src="https://i.imgur.com/62pYXcI.png" title="source: imgur.com" /></div>

5) O Swagger Editor perguntará se você deseja converter o código JSON em YAML. Clique em OK para converter.

<div align="center"><img src="https://i.imgur.com/M6aO4li.png" title="source: imgur.com" /></div>

6) No Menu **Edit**, selecione a opção **Convert to OpenAPI3**

<div align="center"><img width="850px" src="https://i.imgur.com/QOsixbb.png" title="source: imgur.com" /></div>

7) Em seguida, clique na guia **Convert**

<div align="center"><img width="850px" src="https://i.imgur.com/iAjyPZ3.png" title="source: imgur.com" /></div>

8) No menu **Generate Client**, selecione a opção **html2**.

<div align="center"><img width="850px" src="https://i.imgur.com/5lBdwel.png" title="source: imgur.com" /></div>

9) O <b>Swagger Editor</b> solicitará o download do arquivo <b>html2-client-generated.zip</b>. Faça o download do arquivo, descompacte no seu computador e abra o arquivo <b>index.html</b> no seu navegador.

<div align="center"><img  src="https://i.imgur.com/gwLVSIU.png" title="source: imgur.com" /></div>

10 No seu navegador, no menu principal, clique em <b>Imprimir</b>. 

<div align="center"><img width="900px" src="https://i.imgur.com/aLXPYQn.png" title="source: imgur.com" /></div>

11) Na janela de impressão, no item <b>Destino</b>, selecione a opção <b>Salvar em PDF</b> e clique no Botão <b>Salvar</b>.

<div align="center"><img width="900px" src="https://i.imgur.com/TH238QQ.png" title="source: imgur.com" /></div>

12) Documentação em PDF gerada!

<div align="center"><img width="900px" src="https://i.imgur.com/UYOulUu.png" title="source: imgur.com" /></div>

