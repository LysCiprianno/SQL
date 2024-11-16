# Repositório de Consultas SQL para Indicados ao Oscar
 
Este repositório contém um conjunto de consultas SQL para responder a perguntas sobre os indicados ao Oscar, utilizando uma base de dados com informações dos nomeados e vencedores de diversas edições do prêmio. As consultas são feitas sobre uma tabela chamada `indicados_ao_oscar`, que contém dados como o nome dos indicados, o filme ao qual foram indicados, a categoria, o ano da cerimônia e se o indicado foi vencedor ou não.
 
## Estrutura da Base de Dados
 
A tabela `indicados_ao_oscar` tem a seguinte estrutura:
 
- **ano_filmagem**: Ano em que o filme foi filmado.
- **ano_cerimonia**: Ano da cerimônia do Oscar.
- **cerimonia**: Número da cerimônia do Oscar.
- **categoria**: Categoria da indicação.
- **nome_do_indicado**: Nome do indicado.
- **nome_do_filme**: Nome do filme ao qual o indicado foi nomeado.
- **vencedor**: Indica se o indicado foi o vencedor na categoria (valor booleano).
 
## Perguntas Respondidas

```sql
 1. Quantas vezes Natalie Portman foi indicada ao Oscar?

Resposta: 3 vezes.  
Consulta SQL:

SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_do_indicado LIKE '%Natalie Portman%';
 
2. Quantos Oscars Natalie Portman ganhou?
 
Resposta: Apenas 1 Oscar.
Consulta SQL:
 
SELECT COUNT(*)
FROM indicados_ao_oscar
WHERE nome_do_indicado LIKE '%Natalie Portman%' AND vencedor = 'true';
 
3. Amy Adams já ganhou algum Oscar?
 
Resposta: Não, Amy nunca ganhou o Oscar.
Consulta SQL:
 
SELECT COUNT(*) > 0
FROM indicados_ao_oscar
WHERE nome_do_indicado LIKE '%Amy Adams%' AND vencedor = 'true';
 
4. A série de filmes Toy Story ganhou um Oscar em quais anos?
 
Resposta: Em 2011 e 2020.
Consulta SQL:
 
SELECT DISTINCT ano_cerimonia
FROM indicados_ao_oscar
WHERE vencedor = 'true' AND nome_do_filme LIKE '%Toy Story%';
 
5. A partir de que ano a categoria "Actress" deixa de existir?
 
Resposta: Em 1976.
Consulta SQL:
 
SELECT ano_cerimonia
FROM indicados_ao_oscar
WHERE categoria = 'Actress'
ORDER BY ano_cerimonia DESC
LIMIT 1;
 
6. Quem ganhou o primeiro Oscar para Melhor Atriz? Em que ano?
 
Resposta: Marie-Christine Barrault, em 1977.
Consulta SQL:
 
SELECT nome_do_indicado, ano_cerimonia
FROM indicados_ao_oscar
WHERE categoria = 'ACTRESS IN A LEADING ROLE'
ORDER BY ano_cerimonia
LIMIT 1;
 
7. Alterar os valores de "Vencedor" para 1 e 0
 
Consulta SQL:
 
UPDATE indicados_ao_oscar
SET vencedor = 1 WHERE vencedor = 'true';
UPDATE indicados_ao_oscar
SET vencedor = 0 WHERE vencedor = 'false';
 
8. Em qual edição do Oscar "Crash" concorreu ao Oscar?
 
Resposta: Em 2006.
Consulta SQL:
 
SELECT ano_cerimonia
FROM indicados_ao_oscar
WHERE nome_do_filme = 'Crash'
LIMIT 1;
 
9. O filme Central do Brasil apareceu no Oscar?
 
Resposta: Sim, ganhou.
Consulta SQL:
 
SELECT COUNT(*) > 0
FROM indicados_ao_oscar
WHERE nome_do_filme LIKE '%Central Station%';
 
10. Incluir no banco 3 filmes que nunca foram nomeados ao Oscar, mas que merecem ser.
 
Consulta SQL:
 
INSERT INTO indicados_ao_oscar
(ano_filmagem, ano_cerimonia, cerimonia, categoria, nome_do_indicado, nome_do_filme, vencedor)
VALUES
(2024, 2025, 97, 'BEST PICTURE', 'Coralie Fargeat, Demi Moore, Margaret Qualley', 'The Substance', 1),
(2024, 2025, 97, 'ACTRESS IN A LEADING ROLE', 'Demi Moore', 'The Substance', 1);
 
11. Denzel Washington já ganhou algum Oscar?
 
Resposta: Não, nunca ganhou o Oscar.
Consulta SQL:
 
SELECT COUNT(*) > 0
FROM indicados_ao_oscar
WHERE nome_do_indicado LIKE '%Denzel Washington%' AND vencedor = 'true';
 
12. Quais filmes ganharam o Oscar de Melhor Filme?
 
Resposta: Uma lista extensa de filmes que ganharam o Oscar de Melhor Filme, como Lawrence of Arabia, Titanic, Forrest Gump, entre outros.
Consulta SQL:
 
SELECT DISTINCT nome_do_filme
FROM indicados_ao_oscar
WHERE categoria = 'BEST PICTURE' AND vencedor = 1;
 
Bonus: Quais filmes ganharam o Oscar de Melhor Filme e Melhor Diretor na mesma cerimônia?
 
Resposta: Muitos filmes já ganharam o Oscar, mas os meus preferidos são: The Godfather (O Poderoso Chefão), Casablanca, Gone with the Wind (E o Vento Levou) e The Lord of the Rings: The Return of the King (O Senhor dos Anéis: O Retorno do Rei). Esses filmes marcaram não só a história do cinema, mas também deixaram uma forte impressão pessoal. 💗
Consulta SQL:
 
SELECT DISTINCT nome_do_filme
FROM indicados_ao_oscar
WHERE vencedor = 1 AND categoria = 'BEST PICTURE'
AND nome_do_filme IN (  
  SELECT nome_do_filme  
  FROM indicados_ao_oscar  
  WHERE categoria = 'DIRECTING' AND vencedor = 1);
 
Bonus: Denzel Washington e Jamie Foxx já concorreram ao Oscar no mesmo ano?
 
Resposta: Não, nunca.
Consulta SQL:
 
SELECT COUNT(*) > 0
FROM indicados_ao_oscar AS a
JOIN indicados_ao_oscar AS b
  ON a.ano_cerimonia = b.ano_cerimonia
WHERE a.nome_do_indicado = 'Denzel Washington'
  AND b.nome_do_indicado = 'Jamie Foxx';
