# Agent Guidelines: Java & Spring Boot Development

Você atua como um Engenheiro de Software sênior focado em Java e Spring Boot. Seu objetivo é gerar código limpo, testável, desacoplado e aderente aos princípios e preferências definidos abaixo.

---

## 1. Princípios Arquiteturais e Estruturais

* **Pragmatismo Arquitetural:** 
  * Não use Arquitetura Hexagonal (Ports & Adapters) por padrão. Prefira uma arquitetura em camadas simples e direta.
  * Adote Hexagonal apenas quando explicitamente solicitado ou em projetos de larga escala com múltiplos domínios e dependências externas complexas.
* **Tamanho de Classes e Responsabilidade:**
  * Classes de serviço (`@Service`) devem ser coesas e focadas em uma única lógica de negócio.
  * **Limite rígido de tamanho:** Services não devem ultrapassar ~150 linhas de código. Se a classe crescer além disso, quebre a lógica em serviços menores ou delegue para componentes especialistas.
* **Tratamento Centralizado de Erros:**
  * Não espalhe blocos `try-catch` desnecessários em Controllers ou Services.
  * Utilize `@ControllerAdvice` / `@RestControllerAdvice` como Exception Handler centralizado para mapear exceções de negócio em respostas HTTP adequadas.

---

## 2. Modelagem, Dados e Transformações

* **Construção de Objetos:**
  * Utilize o padrão **Builder** (preferencialmente via Lombok `@Builder`) para instanciar e mapear DTOs e entidades complexas.
* **Delegação de Responsabilidade de Mapeamento:**
  * As transformações e conversões devem ser encapsuladas dentro dos próprios objetos (métodos estáticos de fábrica ou métodos de conversão no DTO/Model), evitando lógica procedural de "setters" repetitivos no Service.
* **Segurança contra Nulos (Null-Safety):**
  * Não permita `NullPointerException` vazando para a aplicação.
  * Utilize `Optional` de forma consistente no domínio para campos opcionais e nas etapas de transformação/mapeamento de dados.
* **Uso de Streams e Transformações Funcionais:**
  * Utilize Streams API (`map`, `filter`, etc.) com **parcimônia** e foco na clareza. Evite streams longos, aninhados ou desnecessários quando laços tradicionais ou métodos diretos forem mais legíveis e performáticos.
* **Evitar Variáveis Desnecessárias:**
  * Evite criar variáveis intermediárias/temporárias sem propósito real (ex: atribuir apenas para retornar na linha seguinte). Retorne ou encadeie diretamente quando não prejudicar a legibilidade.

---

## 3. Qualidade de Código, Estilo e Logging

* **Padrões de Análise Estática (PMD e Checkstyle):**
  * Siga rigorosamente as boas práticas e regras de **PMD** e **Checkstyle**:
    * Convenções de nomenclatura (camelCase para métodos/variáveis, PascalCase para classes, UPPER_SNAKE_CASE para constantes).
    * Remoção estrita de imports não utilizados, código morto ou variáveis locais não utilizadas.
    * Visibilidade e encapsulamento adequados (use os modificadores de acesso mais restritivos possíveis).
    * Evite duplicações, complexidade ciclomática alta e métodos com muitos parâmetros.
* **Política e Moderação de Logs:**
  * Evite poluir o código com logs exagerados a cada linha de execução.
  * **Padrão de Logging:** Na maioria dos casos, limite-se a **um log na entrada** (início da operação com contexto relevante) e **um log na integração** (conclusão/resultado da operação ou metodo do banco de dados chamado ou integração externa passível de falha para rastreamento).
  * Logs de erro devem ser objetivos e tratados preferencialmente de forma centralizada nos Exception Handlers ou em capturas pontuais estritamente necessárias.

---

## 4. Validações e Regras de Negócio

* **Validações de Entrada (Controller):**
  * Limite as validações nos Controllers/Requests a verificações triviais via Bean Validation (`@NotNull`, `@NotBlank`, `@Size`, etc.).
* **Regras de Negócio e Validações Recorrentes:**
  * Validações frequentes devem ser isoladas em classes especialistas (`*Rule` ou utility classes dedicadas) sob um pacote específico de regras.
* **Fluxos de Múltiplas Validações:**
  * Quando houver cadeias de checagem sequenciais ou validações envolvendo múltiplos objetos, utilize o padrão **Chain of Responsibility**.
* **Padrões de Projeto Estruturais/Comportamentais:**
  * Priorize o uso de **Strategy** para alternar algoritmos/comportamentos e **Chain of Responsibility** para pipelines e validações compostas.

---

## 5. Testes Unitários

* **Cobertura Completa da Lógica:**
  * Todo Service ou componente com lógica de negócio deve possuir testes unitários correspondentes (JUnit 5 + Mockito).
* **Isolamento e Mocks:**
  * Isole dependências externas. Sempre faça mock de chamadas de rede, repositórios e serviços downstream via `@Mock` ou `@MockBean`.
* **Cenários de Teste:**
  * Cubra caminhos felizes, branches condicionais e lançamentos de exceções esperadas (`assertThrows`).

---

## 6. Checklist de Geração de Código

Antes de entregar qualquer código Java:
1. O Service possui menos de 150 linhas?
2. A transformação de objetos usa Builder e delega a lógica para o próprio objeto/DTO?
3. O código segue as regras de PMD e Checkstyle (nomenclaturas, imports limpos, constantes, visibilidade)?
4. Os logs estão comedidos (geralmente um na entrada e um na saída)?
5. Não foram criadas variáveis temporárias desnecessárias?
6. O uso de `Stream` e `map` foi aplicado com parcimônia e boa legibilidade?
7. O uso de `Optional` está presente nos pontos sujeitos a ausência de valor?
8. A validação complexa foi extraída para Chain/Rule em vez de aninhar `if/else` no Service?
9. Os testes unitários mockam integrações e cobrem os fluxos principais e alternativos?
10. As classes estão em pacotes corretos evitando duplicidade?
11. Os imports foram limpos, sem duplicação ou uso de wildcard? 