# Técnicas

## Bean Validation
É interessante usar anotações para validar unicidade e existência no banco de dados, porém ao longo tempo se torna um problema, pois você fica limitado de poder usar somente uma anotação de unicidade/existencial. Se for necessário uma validação mais complexa pode acabar lhe atrapalhando.

Para fazer uma anotação primeiro começamos criando uma interface da anotação:
```java
@Target(ElementType.FIELD) // Locais onde poderam funcionar, nesse caso só em campos
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = GenericExistenceValidator.class)
public @interface UMA_ANOTACAO {
    String message() default "UMA_MESSAGEM_AQUI";
    Class<?>[] groups() default {};
    Class<? extends Payload>[] payload() default {};

    // Parametros que serão utilizado
    // ...
}
```

E após fazer um validador que implemente o _ConstraintValidator_.
```java
public class UM_VALIDADOR implements ConstraintValidator<UMA_ANOTACAO, Object> {

    // Atributos que precisam ser armazenados

    @Override
    public void initialize(UMA_ANOTACAO params) {
        // Inicializa os parametros
    }

    @Override
    public boolean isValid(Object obj, ConstraintValidatorContext context) {
        // A validação
    }
}

```

E após é só utilizar sua anotação.

## Sobre Orientação a objeto

Não se pode ficar digitando código com o paradigma procedural em uma linguagem totalmente orientada a objeto, isto é, fazer operações com atributos encapsulados fora da classe, além de acoplar uma funcionalidade importante da classe que deveria saber fazer tal operação a complexidade do código aumenta.

Se uma classe Soma possui os atributos que serão somados, o mais óbvio é ter uma operação calcular, que quando chamada seja executado internamente e retorne somente resultado.

## Entity Maneger

O JPA usa esta classe para fazer as operações com o banco de dados, então é necessário saber como ela funciona para quando houver algum erro já saber o porquê.

É necessário abrir uma transação toda vez que for fazer alguma operação no banco, para isso existe a anotação _@Transactional_, marque com essa anotação no seu controller e a transação já vai estar aberta.

Para salvar classes aninhadas, é necessário que todas as parte que o possuem aninhamento e sejam não-nulo estejam atribuídas devidamente. Caso contrário o será lançado um expection para de erro com campo nulo no banco.

## Health Check 

Implementar um health check é muito importante para monitorar a saúde da aplicação, para isso é feito um endpoint que retorna um status da aplicação.
O Spring Actuator age justamente em cima disso, quando adicionado ao pom é criado vários endpoint que indicam o estado da aplicação.
Para adicioná-lo usa-se o seguinte XML

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
```

Para acessar esses endpoint deve-se fazer a requisição para `{endereço-do-host}/actuator/{endpoint}`, a tabela de endpoints está disponível [aqui](https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-endpoints).

[Voltar](../README.md)