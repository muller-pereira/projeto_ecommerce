# Projeto de SQL para Análise de Vendas do E-commerce Brasileiro

## Visão Geral

Este projeto visa aprimorar habilidades em SQL através da criação, manipulação e consulta de tabelas em um banco de dados PostgreSQL. Utilizando um dataset público, 
realizaremos uma análise exploratória de dados (EDA) para identificar padrões e obter insights valiosos sobre as vendas de um e-commerce brasileiro.

## Objetivos
* **Criação de Tabelas:** Configurar um banco de dados PostgreSQL com as tabelas necessárias para armazenar os dados.

* **Análise Exploratória de Dados (EDA):** Identificar dados nulos ou duplicados e realizar consultas para obter insights sobre as vendas.

### Tecnologias Utilizadas

* SQL
* [Git](https://git-scm.com/downloads)
* [PostgreSQL](https://www.postgresql.org/download/?form=MG0AV3)
* [Microsoft Power BI (opcional para visualizações)](https://learn.microsoft.com/pt-br/power-bi/)

### Dependências e Versões Necessárias

* Git - Versão: 2.47.1.windows.2
* pgAdmin (PostgreSQL) - Versão: 8.12
* Microsoft Power BI Desktop - Versão: 2.139.1678.0 64-bit (janeiro de 2025)

## Dados

* **Fonte:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

* **Descrição:** O dataset contém informações sobre pedidos feitos em um e-commerce brasileiro, incluindo detalhes sobre vendas, produtos, clientes e geolocalização.

## Como rodar o projeto ✅

Para rodar este projeto, você precisará ter o Git e o PostgreSQL instalados em sua máquina.

1. **Instalção do PostegreSQL:**
    
    * Siga as instruções de instalação para o seu sistema operacional no site oficial do [PostgreSQL](https://www.postgresql.org/download/?form=MG0AV3).

2. **Configurar o banco de dados e criar o schema**

    * **Criar o banco de dados**
    1.  Abra o terminal do PostgreSQL ou o pgAdmin. Dica: Utilize o Git Bash.
    2. No terminal, conecte-se ao PostgreSQL:

        ```
        psql -U postgres
        ```
    3. Crie um banco de dados chamado ecommerce:

        ```
        CREATE DATABASE ecommerce;
        ```
    * **Criar um Schema**
    1. Conecte-se ao banco de dados ecommerce:
        ```
        \c ecommerce
        ```    
    2. Crie um schema chamado olist:
        ```
        CREATE SCHEMA olist;
        ```     
3. Criar as tabelas no schema olist

    * **Criar as tabelas** \
    Use os comandos SQL para criar cada tabela dentro do schema olist. Certifique-se de que o schema está ativo, ou sempre prefixe as tabelas com o schema.\
    Exemplo:
    ```
    SET search_path TO olist;

    CREATE TABLE order_payments (
    order_id VARCHAR(50) NOT NULL,
    payment_sequential INTEGER NOT NULL,
    payment_type VARCHAR(20) NOT NULL,
    payment_installments INTEGER NOT NULL,
    payment_value NUMERIC(10, 2) NOT NULL,
    PRIMARY KEY (order_id, payment_sequential)
    );
    ```
    **Nota:**
    Repita o processo para todas as tabelas. Para simplificar, copie e cole os comandos SQL das definições fornecidas anteriormente.

4. **Importar os dados do CSV**
    * **Configurar o arquivo CSV**
    1. Coloque os arquivos CSV (baixados do Kaggle ou na pasta olist deste projeto) em um diretório acessível ao PostgreSQL.
    2. Certifique-se de que os nomes das colunas nos CSV correspondem aos nomes das colunas nas tabelas.
    * **Importar os dados**
    No terminal ou no pgAdmin, use o comando COPY para importar os dados. Por exemplo:
        ```
        COPY olist.order_payments(order_id, payment_sequential, payment_type, payment_installments, payment_value)
        FROM '/caminho/para/olist_order_payments_dataset.csv'
        DELIMITER ','
        CSV HEADER;
        ```
        **Nota:** 
        *   Substitua /caminho/para/ pelo caminho completo para o arquivo CSV.
        * Certifique-se de que o PostgreSQL tenha permissão para acessar o diretório.
        
5. **Verificar os dados**\
Após importar os dados, verifique se foram carregados corretamente:
    ```
    SELECT * FROM olist.order_payments LIMIT 10;
    ```

6. **Instrução de uso (Git Bash)**
    * **Clone o repositório:**
    Abra o Git Bash e execute o comando
        ```
        git clone https://github.com/muller-pereira/projeto_ecommerce.git
        ```
## Tratamento de Dados

1. Verficar se existem dados duplicados ou nulos em cada tabela. Caso necessário, tratar esses dados.\
Exemplo:
* Duplicados
    ```
    select seller_id, count(seller_id)
    from olist.sellers
    group by seller_id
    having count(seller_id) > 1
    ```
* Nulos 
    ```
    select  count(*)
    from olist.order_reviews
    where review_comment_title is null
    ```

## 📌 Análises Realizadas

As querys (consultas) estão localizadas na pasta querys deste projeto.

1. **Média de Vendas por Mês:** Determinação da média de vendas realizadas por mês.
2. **Categorias de Produtos:** Identificação das 5 categorias que mais venderam e as 5 que menos venderam nos últimos 12 meses de registro.
3. **Desempenho dos Vendedores:** Identificação dos 5 vendedores que mais venderam e os que menos venderam nos últimos 12 meses de registro.
4. **Distribuição Geográfica das Compras:** Identificação dos 5 estados com mais compras e os estados com menos compras.
5. **Market Share:** Cálculo do percentual de market share dos 5 vendedores que mais venderam nos últimos 12 meses de registro.
6. **Média Móvel:** Cálculo da Média Móvel de 3 meses dos últimos 12 meses da quantidade de vendas das 5 categorias mais vendidas.

## Resultados  | :memo:
* A **média de vendas mensais** foi de 4194,7, porém o desvio padrão foi de 2426,64, o que indica que os dados estão muito dispersos. Em outras palavras, tiveram meses com muitas vendas e meses com pouquísimas vendas. Isso fica evidenciado na imagem a seguir:

* **Categorias de Produtos que mais venderam:**\
Alguns fatores podem ter contribuído para que essas categorias tivessem mais vendas:
    1. **Cama, Mesa, Banho:** Esses itens são essenciais para qualquer residência, e as pessoas frequentemente investem em móveis de qualidade para o conforto e funcionalidade do lar.
    2. **Beleza e Saúde:** Produtos de beleza e saúde são comprados regularmente, pois fazem parte da rotina diária e dos cuidados pessoais. A demanda constante por esses produtos contribui para suas altas vendas.
    3. **Esporte e Lazer:** A popularidade de produtos esportivos e de lazer pode ser atribuída ao aumento do interesse em atividades físicas e hobbies. Muitas pessoas buscam melhorar seu bem-estar e se divertir em seu tempo livre.
    4. **Informática e Acessórios:** Nos dias de hoje, a tecnologia é uma parte central da vida cotidiana. Produtos como laptops, tablets, acessórios e periféricos são frequentemente atualizados, levando a uma alta demanda.
    5. **Móveis e Decoração:** A decoração do lar é uma forma de expressão pessoal e muitas pessoas gostam de renovar e personalizar seus espaços. Produtos de móveis e decoração são populares para criar ambientes acolhedores e estilosos.
* **Categorias de Produtos que menos venderam:**\
Esses fatores podem ter contribuído para que essas categorias tivessem menos vendas:
    1. **Seguros e Serviços:** Esses produtos são comprados com pouca frequência, pois os serviços são contratados por períodos longos, como anualmente, e a decisão de contratar um seguro pode ser complexa.
    2. **PC Gamer:**  Produtos para gamers têm um público específico e vendem menos em comparação com categorias mais amplas. Seu preço elevado pode limitar o número de compradores.
    3. **Fashion - Roupas Infantojuvenis:** A demanda por roupas infantis e juvenis é sazonal e depende da idade das crianças e de suas preferências de estilo.
    4. **CDS e DVDs Musicais:** Com o crescimento do streaming, a demanda por CDs e DVDs caiu, pois muitos preferem a conveniência do streaming.
    5. **La Cuisine:** Produtos de cozinha podem ter vendas baixas sem promoção ou alta demanda, e são comprados menos frequentemente, pois as pessoas investem em utensílios de qualidade que duram mais.

* **Desempenho dos vendedores**\
Percebe-se que há uma enorme diferença entre os valores das vendas dos 5 primeiros vendedores e dos 5 últimos. Isso pode indicar que esses últimos são novos, têm menos visibilidade ou estão vendendo produtos menos populares.

* **Distribuição Geográfica das Compras:**
    1. **Distribuição Geográfica:** Os estados com mais compras estão localizados principalmente no sul e sudeste do Brasil, que são regiões mais desenvolvidas e com maior densidade populacional. Em contraste, os estados com menos compras estão localizados na região norte, que enfrenta desafios logísticos e menor densidade populacional.
    2. **Volume de Compras:** Há uma diferença significativa no número de compras entre os estados com mais e menos compras. São Paulo, por exemplo, tem mais de 31.000 compras, enquanto Roraima tem apenas 28 compras. Isso pode ser atribuído a fatores como população, acesso à internet e infraestrutura.
    3. **Potencial de Crescimento:** Os estados com menos compras têm potencial para crescimento. Investir em melhorias de infraestrutura, marketing direcionado e parcerias locais pode ajudar a aumentar as vendas nessas áreas.

* **Market Share:**\
Market share, ou participação de mercado, é a porcentagem das vendas ou receitas totais de um vendedor em relação às vendas ou receitas totais.
A participação de mercado mostra uma diferença pequena. Todos os vendedores têm um bom desempenho, mas há espaço para crescimento, especialmente para os vendedores com menor participação de mercado.

* **Média Móvel:**\
Em estatística, a média móvel é um recurso utilizado para se identificar a tendência de 
um conjunto de dados dispostos em uma série de tempo. 
A média móvel de três meses é a soma 
dos valores das vendas dos últimos três meses divido por 3. 
    1. **Média Móvel Inicial**\
    Nos primeiros dois meses de cada categoria (2017-09 e 2017-10), a média móvel é 0. Isso ocorre porque são necessários três meses para começar a calcular uma média móvel de janela 3 (ou seja, baseando-se em três períodos anteriores).
    2. **Tendências**
        * **Beleza e Saúde:** Vendas aumentam de forma consistente até julho de 2018, com a média móvel acompanhando o crescimento. Isso indica uma tendência clara de crescimento.
        * **Cama, Mesa e Banho:** Flutuações são mais visíveis, especialmente em janeiro de 2018, onde as vendas aumentam significativamente, mas a média móvel consegue suavizar essas variações.
        * **Esporte e Lazer:** As vendas atingem um pico em março de 2018 e depois declinam. A média móvel reflete esse comportamento, mas com uma suavização visível.
        * **Informática e Acessórios:** Após um pico em fevereiro de 2018, as vendas apresentam uma tendência de declínio, mas a média móvel suaviza essas quedas.
        * **Móveis e Decoração:** Há flutuações significativas, com um aumento inicial e depois um declínio. A média móvel ajuda a identificar a tendência geral.
    3. **Tendência Geral:** Percerbe-se um pico geral na média móvel dessas categorias entre os meses de dezembro de 2017 e janiero de 2018 e entre fevereiro e março de 2018. Isso pode ter ocorrido devido a alguma promoção na plataforma durante esse período.

## Dashboard


## Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues ou enviar pull requests com melhorias e correções.

## Licença

Este projeto está licenciado sob a Licença MIT - veja o arquivo [LICENSE](https://github.com/muller-pereira/projeto_scraping/blob/main/LICENSE) para mais detalhes.
