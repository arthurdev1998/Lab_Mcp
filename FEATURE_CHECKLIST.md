# 📋 Checklist de Início de Feature

> **⚠️ Este checklist deve ser seguido ANTES de iniciar qualquer desenvolvimento de feature**

## 📖 Leitura Obrigatória

- [x] **PLANO_DE_ACAO.md** - Li o plano completo e entendi as fases do projeto
- [x] **PLANO_TECNICO.md** - Li a arquitetura e padrões técnicos estabelecidos
- [x] **README.md** - Entendi a visão geral e setup do projeto

## 🎯 Planejamento da Feature

### Análise de Domínio
- [x] Identifiquei o **Bounded Context** da feature
- [x] Defini quais **Aggregates** serão afetados
- [x] Identifiquei **Entities** e **Value Objects** necessários
- [x] Defini **Domain Services** se aplicável
- [x] Identifiquei **Domain Events** que serão disparados

### Análise de Aplicação (CQRS)
- [x] Listei **Commands** necessários (operações de escrita)
- [x] Listei **Queries** necessários (operações de leitura)
- [x] Defini **DTOs** de entrada e saída
- [x] Identifiquei **Handlers** que serão implementados
- [x] Defini **Validações** necessárias (FluentValidation)

### Análise de Infraestrutura
- [x] Identifiquei **Repository** interfaces necessárias
- [x] Defini **Entity Configurations** para EF Core
- [x] Identifiquei **External Services** a serem integradas
- [x] Planejei **Migrations** de banco se necessário

## 🏗️ Verificação Arquitetural

### Clean Architecture
- [x] Feature segue separação de camadas (Domain → Application → Infrastructure → Presentation)
- [x] Dependências apontam para dentro (Dependency Inversion)
- [x] Não há vazamento de abstrações entre camadas
- [x] Domain Layer não depende de infraestrutura

### DDD (Domain Driven Design)
- [x] Regras de negócio estão no Domain Layer
- [x] Aggregates mantêm consistência interna
- [x] Value Objects são imutáveis
- [x] Domain Services contêm lógica que não pertence a entidades

### SOLID Principles
- [x] **SRP**: Cada classe tem uma única responsabilidade
- [x] **OCP**: Extensível sem modificar código existente
- [x] **LSP**: Implementações são substituíveis
- [x] **ISP**: Interfaces são específicas e coesas
- [x] **DIP**: Dependências são invertidas via abstrações

### CQRS Pattern
- [x] Commands modificam estado (write operations)
- [x] Queries não modificam estado (read operations)
- [x] Handlers são específicos para cada comando/query
- [x] DTOs são adequados para cada operação

## 🧪 Planejamento de Testes

### Testes Unitários
- [x] **Domain Tests**: Testes para entidades, value objects e domain services
- [x] **Application Tests**: Testes para handlers, validators e use cases
- [x] **Infrastructure Tests**: Testes para repositories e external services
- [x] Cobertura planejada: **> 80%**

### Testes de Integração
- [x] Testes end-to-end da API
- [x] Testes de integração com banco de dados
- [x] Testes de integração com services externos

### Casos de Teste
- [x] **Happy Path**: Cenários de sucesso
- [x] **Edge Cases**: Casos extremos e limites
- [x] **Error Cases**: Cenários de erro e exceções
- [x] **Business Rules**: Validação de regras de negócio

## 🔒 Segurança e Validação

### Input Validation
- [ ] Validações implementadas com **FluentValidation**
- [ ] Sanitização de inputs quando necessário
- [ ] Validação de tipos e formatos
- [ ] Validação de regras de negócio

### Authorization
- [ ] Verificação de permissões necessárias
- [ ] Controle de acesso baseado em roles/claims
- [ ] Validação de ownership de recursos
- [ ] Audit trail se necessário

### Error Handling
- [ ] Try-catch adequado em handlers
- [ ] Exceptions customizadas definidas
- [ ] Error messages user-friendly
- [ ] Logging de erros estruturado

## 📊 Observabilidade

### Logging (Serilog)
- [ ] Logs de **informação** em operações importantes
- [ ] Logs de **warning** em situações suspeitas
- [ ] Logs de **error** em exceções
- [ ] Logs estruturados com contexto relevante
- [ ] Correlation IDs para tracking

### Monitoring
- [ ] Métricas de performance definidas
- [ ] Health checks atualizados se necessário
- [ ] Alertas configurados para falhas críticas

## 📝 Documentação

### Código
- [ ] Comentários em lógica complexa
- [ ] XML Documentation em APIs públicas
- [ ] README atualizado se necessário
- [ ] Exemplos de uso documentados

### API Documentation
- [ ] **Swagger** annotations adicionadas
- [ ] Request/Response examples
- [ ] Error codes documentados
- [ ] Authentication requirements

## 🚀 Implementação

### Development Workflow
- [x] Branch criada seguindo padrão: `feature/FEAT-XXX-description`
- [x] Commits atômicos e descritivos
- [x] Code review interno antes do PR
- [x] All tests passing

### Performance
- [ ] Considerações de performance avaliadas
- [ ] Queries de banco otimizadas
- [ ] Caching implementado se necessário
- [ ] Pagination em listas grandes

### Frontend (React)
- [ ] Componentes seguem padrão estabelecido
- [ ] State management adequado (Redux)
- [ ] Error boundaries implementados
- [ ] Loading states definidos
- [ ] Responsive design verificado

## ✅ Checklist Final

### Antes do Commit
- [ ] Todos os testes unitários passando
- [ ] Code coverage > 80%
- [ ] Análise estática sem issues críticos
- [ ] Performance verificada
- [ ] Documentação atualizada

### Antes do Pull Request
- [ ] Testes de integração passando
- [ ] Feature testada manualmente
- [ ] Error scenarios validados
- [ ] Documentation review
- [ ] Self code review realizado

### Deploy Checklist
- [ ] Environment variables configuradas
- [ ] Database migrations prontas
- [ ] Rollback plan definido
- [ ] Monitoring configurado
- [ ] Load testing se necessário

---

## 🎯 Comando para Criar Feature

```bash
# 1. Ler documentação
echo "Lendo PLANO_TECNICO.md..."

# 2. Criar branch
git checkout -b feature/FEAT-XXX-description

# 3. Implementar seguindo checklist
echo "Implementando feature seguindo Clean Architecture + DDD..."

# 4. Executar testes
dotnet test --collect:"XPlat Code Coverage"

# 5. Verificar qualidade
dotnet build --no-restore --verbosity normal

# 6. Criar PR
git push origin feature/FEAT-XXX-description
```

## 📞 Em Caso de Dúvidas

1. **Revisar PLANO_TECNICO.md** - Primeira fonte de verdade
2. **Consultar código existente** - Exemplos de implementação
3. **Verificar testes** - Como features similares foram testadas
4. **Documentação oficial** - .NET, EF Core, React, etc.

---

**🚨 IMPORTANTE**: Este checklist não é opcional. Seguir estes passos garante consistência, qualidade e manutenibilidade do código!
