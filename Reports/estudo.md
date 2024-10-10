# 📚 Relatório de Estudos

**Nome do Estagiário:** Pablo Vinícius Domingues Sanches  
**Data:** 09/10/2024  

---

## 📑 Conteúdo:

- [**Governança de Dados**](#gdados)
- [**Dataplex**](#dtplex)

---

## 1. Governança de Dados <a id="gdados"></a>

Governança de dados refere-se à definição de políticas e padrões internos para gerenciar como os dados são coletados, armazenados, processados e descartados. Ela garante controle sobre o acesso e uso dos dados, assegurando que sejam seguros, privados, precisos, disponíveis e utilizáveis.

> O principal objetivo da governança de dados é garantir que os dados estejam acessíveis, seguros, de alta qualidade e usados de maneira eficiente e ética. 

### **1.1 Qualidade de Dados** 

A qualidade de dados assegura que os dados sejam precisos, completos, consistentes e confiáveis. Ferramentas de qualidade de dados ajudam a identificar e corrigir erros ou inconsistências.

### **1.2 Segurança de Dados** 

Assegura que os dados estejam protegidos contra acessos não autorizados e violações. Isso envolve:

- **`Controle de Acesso:`** Definir quem tem permissão para visualizar, editar ou excluir dados.
- **`Criptografia:`** Proteger dados sensíveis, tanto em trânsito quanto em repouso.

### **1.3 Conformidade e Regulamentações** 

Garantir que os dados sejam gerenciados em conformidade com regulamentações como:

- **`LGPD:`** Proteção de dados pessoais.
- **`SOX:`** Leis para garantir integridade financeira.

### **1.4 Catálogo de Dados** 

Criação de um catálogo que documenta onde estão os dados, como estão estruturados e quem é o responsável por eles. Isso facilita a descoberta e reutilização de dados dentro da organização.

### **1.5 Auditoria e Monitoramento** 

Registro de todas as ações relacionadas aos dados (quem acessou, alterou, excluiu). Esse rastreamento é fundamental para a conformidade com regulamentos e para a detecção de comportamentos anômalos.

### **1.6 Gerenciamento do Ciclo de Vida dos Dados** 

Definir o ciclo de vida dos dados, desde sua criação e armazenamento até sua eventual exclusão ou arquivamento. Isso evita o acúmulo de dados irrelevantes ou obsoletos.

---

## 2. **Dataplex** <a id="dtplex"></a>

O Google Dataplex é uma solução na Google Cloud para governança de dados distribuídos. Ele simplifica a organização, segurança e descoberta de dados em ambientes **multicloud** e **on-premises**. Dataplex combina governança, integração e automação para facilitar o gerenciamento de dados em grande escala.

> - **🌩️ Multicloud:** Refere-se ao uso de mais de uma plataforma de computação em nuvem para diferentes serviços, aplicações ou tarefas dentro de uma mesma organização.

> - **🏢 On-premises:** Refere-se a uma infraestrutura de TI que é mantida localmente, dentro das instalações da empresa, onde servidores, sistemas e dados são gerenciados fisicamente. 

### **2.1 Lakes** 

Um Data Lake no Dataplex é uma coleção de dados armazenada em diferentes formas (estruturados e não estruturados) dentro de um ambiente centralizado, geralmente usando o Cloud Storage. O conceito de lake no Dataplex é amplamente utilizado para referenciar áreas de armazenamento de dados brutos, que podem ser usados para análise posterior.

### **2.2 Zones** 

As **Zones** são subcomponentes dentro de um **Lake** no Dataplex, representando áreas lógicas para organizar dados de acordo com critérios como:

- **`Raw Zone:`** Dados brutos, sem processamento.
- **`Curated Zone:`** Dados refinados e preparados para uso analítico.

### **2.3 Assets** 

Os **Assets** são as unidades de dados gerenciadas dentro de uma **Zone**, podendo incluir datasets do BigQuery, buckets no Cloud Storage ou tabelas que são registradas no Dataplex para governança e análise.

O Dataplex facilita a análise de dados com ferramentas nativas do GCP, como BigQuery, e fornece uma camada de abstração para simplificar o acesso aos dados.

<div style="text-align: center;">
<img src="Images/lakes.png" alt="alt text" width="600" height="255"/>
</div>

---

## 🔗 Links de Laboratórios

- https://www.cloudskillsboost.google/course_templates/726?catalog_rank=%7B%22rank%22%3A1%2C%22num_filters%22%3A1%2C%22has_search%22%3Atrue%7D&search_id=37620328

---

## **🛠️ Recursos Utilizados:**  

- https://github.com/2RP-Squad404/Cloud_Strike/tree/develop/relatorios
- https://youtu.be/Gf_0cqJ4psA?si=RYk__E0acZmlRlp8
- https://youtu.be/eRzx4VSjAXg?si=RZCbz6niLiyEMrf4
- https://youtu.be/_4NjYHOLa6U?si=3YKmGuIV6NRvMWAr