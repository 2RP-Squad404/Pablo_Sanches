# Relatório de Estudos

**Nome do Estagiário:** Pablo Vinícius Domingues Sanches  
**Data:** 07/08/2024

**Módulos/Etapas Feitas:**  
1. **Modelagem de Dados**
2. **Git**
3. **Mensageria**

<br>

## Resumo dos módulos 

 # **1. Modelagem de Dados**
Resumidamente, dados que serão utilizados em um sistema de informação.

### **1.2 Modelo SQL**
É um modelo do tipo de banco de dados relacionais, com o foco nas relações de tabelas, caracterizado por sua qualidade em **queries**(consultas). Porém, é um sistema rígido, que possui uma baixa performace(escabilidade).

**1.2.1 Cardinalidade e Relacionamnetos**<br>
Cardinalidade refere-se à quantidade de instâncias de uma entidade que podem ou devem se relacionar com outra entidade.
<br>

- **1 pra 1**
Exemplo: *1* **professor** possui apenas *1* **curso**
*1* **curso** possui apenas *1* **professor**

<br>

- **1 pra Muitos(N)**
Exemplo: *1* **curso** pode ter *várias* **vendas**
*1* **venda** por **curso**

<br>

- **Muitos pra Muitos**
Exemplo: *1* **aluno** pode ter *vários* **cursos**
*1* **curso** pode ter *vários* **alunos**

<br>

**1.2.2 Normalização**<br>
A normalização é um processo em bancos de dados que visa organizar os dados para reduzir a redundância e melhorar a integridade. O objetivo é dividir grandes tabelas em tabelas menores e definir relacionamentos entre elas. Existem várias formas normais, cada uma com suas próprias regras. As principais são:

- **Primeira Forma Normal (1NF)**
A tabela deve possuir atributos únicos, sem existir valores multivalorados.

<br>

- **Segunda Forma Normal (2NF)**
Atende à 1NF e os atributos dependem apenas da chave primária.

<br>

- **Terceira Forma Normal (3NF):**
Atende à 2NF e os atributos devem ser independentes entre si.

<br>

### **1.3 Modelo NoSQL**
Diferentemente do modelo SQL, aqui temos um modelo muito flexível, se destacando em sua performace(Big Data). Mas ao contrário do modelo SQL, sua utilização para queries complexas, não são eficazes.
   

 # **2. Git**
 - ***Git:*** Sistema de controle de versão.
 - ***GitHub:*** Serviço ou plataforma online para a armazenar projetos que utilizam Git.

 Neste módulo foi explorado comandos essencias para melhor aproveitamento do sistema git, facilitação e inspeção de modificações em um projeto. <br>
**Comandos vistos:**\
- `git log`
- `git diff`
- `git reset` 
<br>

### **2.2 Git log**
Em suma, o comando de ***git log*** vai exibir a descrição e o histórico de commits.
Nele é possível ver diversos elementos que compõe um commit:
<br>

- Hash do commit(identicador único);
- Autor do commit;
- Data da realização do commit;
- A mensagem do commit.

<br>
Também é possivel personalizar a saída do ***git log***, com diversas especificações e opções.


### **2.2 Git diff**
O comando ***git diff*** é usado para mostrar as diferenças entre várias versões de arquivos ou entre commits.

### **2.3 Git reset**
Utilizado para desfazer commits, ***git reset*** é essencial para o gerenciamento de versões dentro de um projeto. Ele pode ser aplicado de diferentes maneiras, são elas:

<br>

- `git reset --soft`
Refaz o processo até estar pronto para realizar o commit, já tendo adicionado as alterações.

- `git reset --mixed`
Refaz o processo até o momento de estar preparado para adicionar e ralizar um commit.

- `git reset --hard`
Em suma, tem a função de apagar o commit.

<br>
<br>

**Recursos Utilizados:**  

- Curso Modelagem de Dados (https://youtu.be/SEnnucNP1h0?si=a4IzorMFpTD8d9r-)
- Curso Udemy - Git e GitHub para inciantes (https://www.udemy.com/course/git-e-github-para-iniciantes/learn/lecture/5120580#reviews)
- GitMind (https://gitmind.com/app/docs/mokn7z35)
- VS Code
- Git & GitHub


<br>

**Desafios Encontrados:**  
Para os estudos, tive dificuldade nas pesquisas dos temas e módulos por conta da internet.

**Feedback e Ajustes:**  
No momento, não tenho nenhum feeedback para comentar. 

**Próximos Passos:**  
Contunuarei o estudo no curso de Git e GitHub, e terminarei meu estudo sobre mensageria.



