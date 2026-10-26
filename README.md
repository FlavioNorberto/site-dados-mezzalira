# Desafio de Dados: Metalúrgica Mezzalira

Resolução da **Etapa 1 (Do dado bagunçado ao dado útil)** do Desafio de Dados do processo seletivo do **CITi**.

**Aspirante:** Flávio Norberto

## Sobre o desafio

A Metalúrgica Mezzalira enviou uma planilha de pedidos com vários problemas de qualidade. O objetivo era limpar os dados, registrar tudo o que foi alterado em um log e, no fim, responder:

> Qual cidade mais comprou e em qual mês ela mais gastou?

## Resposta

A cidade que mais comprou foi **Recife**, com **R$ 55.260,00** no total. O mês em que ela mais comprou foi **março**, com **R$ 29.730,00**.

## Abordagem: arquitetura em camadas

O projeto segue a ideia de camadas (medalhão), em que cada etapa parte da anterior.

| Camada | O que faz |
|---|---|
| 🥉 **Bronze** | Lê a planilha original sem alterar nada, tudo como texto. |
| 🥈 **Prata** | Limpa e padroniza os dados, registrando cada correção em um log. |
| 🥇 **Ouro** | Agrega os dados tratados para responder à pergunta do desafio. |

## Problemas encontrados e tratados

Os dados originais têm 60 linhas. Depois da limpeza, ficam 53 pedidos válidos.

| Problema | Tratamento | Linhas afetadas |
|---|---|---|
| Linhas vazias | Removidas | 3 |
| Pedidos duplicados | Mantida a primeira ocorrência de cada número de pedido | 4 |
| Cidade escrita de jeitos diferentes (`recife`, `REC`) | Padronizada para `Recife` | 10 |
| Valores digitados como texto (`R$ 1.234,50`) | Convertidos para número | 7 |
| Datas em formatos diferentes | Padronizadas para `dd/mm/aaaa` | 17 |

### Uma observação sobre as datas

Na planilha, 36 datas já eram datas válidas do Excel, exibidas em `dd/mm/aaaa`. O `00:00:00` que aparece ao ler com pandas (`dtype=str`) é só efeito da leitura, pois o Excel guarda data e hora juntas, com hora zerada. Por isso, essas 36 **não foram contadas como correção**. Foram contadas apenas as datas que estavam em texto fora do padrão:

- 11 no formato `aaaa-mm-dd` (ex.: `2026-01-01`)
- 6 com mês abreviado (ex.: `01-fev-2026`)

## Análises extras

Além da resposta principal, montei outras visões para conhecer melhor o negócio:

- **Faturamento por cliente em cada mês:** mostra quais clientes compram de forma recorrente, quais compram só em meses específicos e quais pararam de comprar.
- **Quantidade de produtos vendidos por mês:** mostra quais produtos mais saem.
- **Quantidade de produtos por cliente:** mostra o que cada cliente costuma buscar.
- **Valor gasto por cliente em cada produto.**

> A planilha não tem coluna de quantidade. Nas análises de produtos, considerei que **cada linha equivale a 1 unidade vendida** (um pedido daquele produto).

## Estrutura do repositório

```
.
├── Desafio_Dados.ipynb                  # Notebook com o passo a passo e as saídas
├── desafio_dados.py                     # Versão em script, para rodar localmente
├── pedidos_metalurgica_mezzalira.xlsx   # Planilha original
└── README.md
```

## Como rodar

**Opção 1: Google Colab**
Abra o `Desafio_Dados.ipynb` no Colab e envie a planilha para o ambiente.

**Opção 2: localmente**

```bash
pip install pandas numpy openpyxl
python desafio_dados.py
```

A planilha precisa estar na mesma pasta do script. Fora do Jupyter/Colab, o script usa `print` no lugar de `display` para mostrar as tabelas.

## Tecnologias

- Python 3
- pandas
- NumPy
- openpyxl
