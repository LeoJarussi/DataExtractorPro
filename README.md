<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DataExtractor Pro - README</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
            background-color: #f4f4f4;
            color: #333;
        }
        .container {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        h1, h2 {
            color: #333;
            border-bottom: 1px solid #ddd;
            padding-bottom: 10px;
        }
        p {
            margin-bottom: 15px;
        }
        code {
            background-color: #e8e8e8;
            padding: 2px 5px;
            border-radius: 4px;
            font-family: "Courier New", Courier, monospace;
        }
        .important-note {
            background-color: #fff3cd;
            border-left: 5px solid #ffeeba;
            padding: 10px;
            margin-bottom: 15px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>DataExtractor Pro</h1>

        <p>Bem-vindo ao DataExtractor Pro, uma solução desenvolvida em Python com o objetivo de simplificar e automatizar o processo de extração de dados tabulares contidos em documentos PDF. Este projeto transforma as tabelas identificadas em arquivos no formato Excel (.xlsx), tornando os dados prontamente acessíveis para análises posteriores, relatórios ou outras manipulações de dados. A ferramenta é particularmente útil para lidar com documentos onde as informações importantes estão encapsuladas em estruturas de tabela, que, de outra forma, exigiriam um esforço manual considerável para serem extraídas e utilizadas.</p>

        <h2>Visão Geral da Solução</h2>
        <p>O DataExtractor Pro opera através de um processo de duas etapas principais, utilizando scripts Python distintos para cada fase. Inicialmente, o primeiro script é responsável por interagir com o arquivo PDF de entrada. Ele utiliza o serviço LlamaParse, uma poderosa ferramenta de análise de documentos, para processar o PDF e extrair seu conteúdo, com uma instrução específica para focar na identificação e isolamento de tabelas. O resultado desta primeira etapa é a geração de arquivos no formato Markdown, onde cada arquivo representa uma página do PDF original, contendo o texto e as tabelas interpretadas pelo LlamaParse.</p>
        <p>Posteriormente, o segundo script entra em ação para processar esses arquivos Markdown. Ele foi cuidadosamente elaborado para analisar o conteúdo Markdown, identificar as estruturas de tabela (que seguem uma formatação específica com o uso de barras verticais como delimitadores) e converter essas tabelas em planilhas Excel. Cada tabela encontrada em cada página Markdown é então salva como um arquivo <code>.xlsx</code> individual, permitindo uma granularidade fina sobre os dados extraídos.</p>

        <h2>Componente de Extração de PDF via LlamaParse</h2>
        <p>A primeira fase do DataExtractor Pro é crucial e depende da biblioteca <code>llama_parse</code>. Para que este componente funcione corretamente, é imprescindível configurar uma chave de API do Llama Cloud. No código fornecido, esta chave é definida diretamente através da variável de ambiente <code>LLAMA_CLOUD_API_KEY</code>. O script instancia o <code>LlamaParse</code> com o tipo de resultado definido como <code>"markdown"</code> e uma instrução de parsing específica: <code>"this file contains text and tables , i would like to get only the tables from the text"</code>. Esta instrução orienta o LlamaParse a priorizar a extração de tabelas. Após o processamento do arquivo PDF de entrada (por exemplo, <code>"resultado.pdf"</code>), o script itera sobre as páginas do documento processado e salva cada uma como um arquivo Markdown separado (por exemplo, <code>meu_pdf/pagina1.md</code>, <code>meu_pdf/pagina2.md</code>, etc.) na subpasta <code>meu_pdf/</code>.</p>

        <h2>Componente de Conversão de Markdown para Excel</h2>
        <p>Uma vez que os arquivos Markdown são gerados, o segundo script do DataExtractor Pro assume a tarefa de conversão. Este script navega pela pasta <code>meu_pdf/</code>, lendo cada arquivo Markdown. Ele emprega expressões regulares para localizar as seções de texto que representam tabelas no formato Markdown. Após a identificação, a biblioteca Pandas é utilizada para interpretar esses dados textuais tabulares. O Pandas lê a tabela, utilizando o caractere de barra vertical <code>|</code> como separador de colunas, e realiza uma limpeza básica, removendo linhas e colunas que possam estar completamente vazias. Finalmente, cada tabela processada é exportada para um novo arquivo Excel (<code>.xlsx</code>) e salva em uma subpasta denominada <code>tabelas/</code>, com nomes de arquivo indicando a página e a sequência da tabela (por exemplo, <code>tabelas/Pagina1Tabela1.xlsx</code>).</p>

        <h2>Requisitos e Dependências do Projeto</h2>
        <p>Para utilizar o DataExtractor Pro, é necessário ter o Python instalado em seu ambiente. Além disso, algumas bibliotecas Python são fundamentais para o funcionamento dos scripts. A primeira é a <code>llama-parse</code>, que é o motor da extração de conteúdo do PDF. A segunda é a <code>pandas</code>, essencial para a manipulação dos dados tabulares e para a criação dos arquivos Excel. Implicitamente, para que o Pandas possa gerar arquivos no formato <code>.xlsx</code>, a biblioteca <code>openpyxl</code> também será necessária e geralmente é instalada como uma dependência do Pandas ou pode precisar ser instalada separadamente. Um ponto crítico é a necessidade de uma chave de API válida para o Llama Cloud, que deve ser configurada no ambiente onde o primeiro script será executado.</p>

        <h2>Instruções de Uso Detalhadas</h2>
        <p>A utilização do DataExtractor Pro envolve seguir alguns passos de configuração e execução. Primeiramente, certifique-se de que todas as dependências Python mencionadas (<code>llama-parse</code>, <code>pandas</code>, <code>openpyxl</code>) estão instaladas em seu ambiente Python. Isso pode ser feito utilizando o gerenciador de pacotes <code>pip</code>. Em seguida, é crucial configurar a sua chave de API do Llama Cloud. No script de extração de PDF, a chave é atribuída à variável de ambiente <code>LLAMA_CLOUD_API_KEY</code>; você precisará substituir o valor de exemplo pela sua chave real.</p>
        <p>Com o ambiente configurado, prepare o arquivo PDF que você deseja processar, por exemplo, nomeando-o como <code>resultado.pdf</code> e colocando-o no mesmo diretório do primeiro script, ou ajustando o caminho no código. Execute o primeiro script Python. Este script criará uma pasta chamada <code>meu_pdf</code> e a populará com arquivos Markdown, um para cada página do seu PDF.</p>
        <p>Após a conclusão bem-sucedida do primeiro script, execute o segundo script Python. Este script procurará os arquivos Markdown na pasta <code>meu_pdf</code>, processará as tabelas contidas neles e criará uma nova pasta chamada <code>tabelas</code>. Dentro desta pasta, você encontrará os arquivos Excel (<code>.xlsx</code>) resultantes, cada um correspondendo a uma tabela extraída.</p>

        <h2>Estrutura de Arquivos e Pastas Gerenciada</h2>
        <p>Durante sua execução, o DataExtractor Pro organiza os arquivos de forma estruturada. O script de extração de PDF cria automaticamente um diretório chamado <code>meu_pdf/</code> no local de execução, onde armazena os arquivos Markdown intermediários. Subsequentemente, o script de conversão para Excel cria outro diretório, <code>tabelas/</code>, também no local de execução, para depositar os arquivos Excel finais. É importante estar ciente dessas criações de pastas para localizar os resultados do processamento.</p>

        <h2>Considerações Importantes e Limitações Atuais</h2>
        <p>É fundamental reconhecer que a eficácia do DataExtractor Pro está intrinsecamente ligada à qualidade do serviço LlamaParse e à clareza da estrutura do PDF original. A precisão na extração de tabelas pode variar dependendo da complexidade do layout do PDF e da formatação das tabelas. Além disso, a detecção de tabelas no segundo script, baseada em expressões regulares, assume um formato Markdown consistente e pode não ser infalível para todas as variações de tabelas. Outro ponto a considerar é a dependência de uma chave de API ativa para o Llama Cloud, sem a qual a primeira etapa do processo não funcionará. Atualmente, o sistema gera múltiplos arquivos de saída (um arquivo Markdown por página e um arquivo Excel por tabela), o que pode representar um grande volume de arquivos para documentos extensos ou com muitas tabelas. O usuário deve estar preparado para gerenciar esses múltiplos arquivos.</p>

        <h2>Conclusão</h2>
        <p>O DataExtractor Pro oferece uma abordagem programática para um desafio comum: a extração de dados tabulares de documentos PDF. Ao combinar o poder do LlamaParse para a análise inicial do PDF e o Pandas para a subsequente transformação em Excel, este projeto fornece uma base sólida para automatizar essa tarefa. Embora existam limitações e dependências a serem consideradas, a ferramenta demonstra um caminho eficaz para desbloquear dados presos em formatos PDF e convertê-los em um formato estruturado e mais útil para diversas aplicações.</p>

    </div>
</body>
</html>