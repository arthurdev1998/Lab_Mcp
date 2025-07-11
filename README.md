# 🚀 PersonalBot - Chatbot de Produtividade Pessoal

## 📋 Sobre o Projeto

PersonalBot é um assistente inteligente de produtividade pessoal que utiliza Llama Local para auxiliar usuários na gestão de tarefas, lembretes, rotinas e otimização de produtividade através de conversas naturais.

## 🎯 Funcionalidades Principais

- ✅ **Gestão de Tarefas**: Criação, edição e organização via chat
- 📅 **Lembretes Inteligentes**: Sistema de notificações personalizadas  
- 📊 **Analytics de Produtividade**: Relatórios e insights sobre performance
- 🤖 **IA Conversacional**: Integração com Llama Local para interações naturais
- 📱 **Interface Moderna**: SPA React responsiva e intuitiva
- 🔄 **Sincronização Real-time**: Updates instantâneos via SignalR

## 🏗️ Arquitetura

### Stack Tecnológico

**Backend (.NET 8)**
- Clean Architecture + DDD
- CQRS com MediatR
- Entity Framework Core
- SignalR para real-time
- FluentValidation
- Serilog para logs estruturados

**Frontend (React 18)**
- TypeScript
- Redux Toolkit para state management
- Material-UI para componentes
- React Query para cache
- Axios para HTTP client

**Database**
- PostgreSQL (produção)
- SQL Server (desenvolvimento)

**AI Integration**
- Llama Local via processo/API
- Prompt Engineering otimizado
- Context Management avançado

## 📁 Estrutura do Projeto

```
PersonalBot/
├── src/
│   ├── PersonalBot.Domain/          # Regras de negócio
│   ├── PersonalBot.Application/     # Casos de uso (CQRS)
│   ├── PersonalBot.Infrastructure/  # Implementações externas
│   ├── PersonalBot.WebAPI/          # Controllers e API
│   └── PersonalBot.UI/              # React SPA
├── tests/                           # Testes automatizados
└── docs/                           # Documentação técnica
```

## 🚀 Getting Started

### Pré-requisitos

- .NET 8 SDK
- Node.js 18+
- PostgreSQL ou SQL Server
- Llama Local configurado
- Docker (opcional)

### Setup Rápido

```bash
# Clone o repositório
git clone https://github.com/arthurdev1998/Lab_Mcp.git
cd Lab_Mcp

# Configure o backend
cd src/PersonalBot.WebAPI
dotnet restore
dotnet ef database update

# Configure o frontend  
cd ../PersonalBot.UI
npm install
npm start

# Execute a API
cd ../PersonalBot.WebAPI
dotnet run
```

## 📚 Documentação

- 📋 [**Plano de Ação**](PLANO_DE_ACAO.md) - Roadmap detalhado do projeto
- 🏗️ [**Plano Técnico**](PLANO_TECNICO.md) - Arquitetura e padrões técnicos
- 🔧 [Setup e Configuração](docs/setup.md)
- 📖 [Documentação da API](docs/api.md)
- 🎨 [Guia de Componentes](docs/components.md)

## 🧪 Testes

```bash
# Testes unitários
dotnet test tests/PersonalBot.Domain.Tests/
dotnet test tests/PersonalBot.Application.Tests/

# Testes de integração
dotnet test tests/PersonalBot.IntegrationTests/

# Testes E2E frontend
cd src/PersonalBot.UI
npm run test:e2e
```

## 📊 Métricas de Qualidade

[![Build Status](https://github.com/arthurdev1998/Lab_Mcp/workflows/CI/badge.svg)](https://github.com/arthurdev1998/Lab_Mcp/actions)
[![Coverage](https://codecov.io/gh/arthurdev1998/Lab_Mcp/branch/main/graph/badge.svg)](https://codecov.io/gh/arthurdev1998/Lab_Mcp)
[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=Lab_Mcp&metric=alert_status)](https://sonarcloud.io/dashboard?id=Lab_Mcp)

- **Cobertura de Testes**: 85%+
- **Performance**: < 2s response time
- **Code Quality**: SonarQube Grade A
- **Disponibilidade**: 99.9%

## 🤝 Contribuindo

1. Leia o [Plano Técnico](PLANO_TECNICO.md) completamente
2. Siga os padrões de Clean Architecture + DDD
3. Implemente testes unitários para nova funcionalidade
4. Execute análise de código estática
5. Crie Pull Request seguindo template

### Convenções de Código

- **C#**: Padrões Microsoft + Clean Code
- **TypeScript**: ESLint + Prettier
- **Commits**: Conventional Commits
- **Branches**: GitFlow

## 🚀 Deploy

### Desenvolvimento
```bash
docker-compose up -d
```

### Produção
- CI/CD com GitHub Actions
- Deploy automatizado no Azure/AWS
- Monitoring com Prometheus + Grafana
- Logs centralizados com ELK Stack

## 📈 Roadmap

### v1.0 (MVP) - Q1 2025
- [x] Arquitetura base implementada
- [ ] CRUD de tarefas via chat
- [ ] Integração Llama básica
- [ ] Interface React funcional

### v1.1 - Q2 2025
- [ ] Analytics de produtividade
- [ ] Notificações push
- [ ] Integração calendário
- [ ] Export de dados

### v2.0 - Q3 2025
- [ ] Machine Learning personalizado
- [ ] Mobile app (React Native)
- [ ] Integração APIs externas
- [ ] Multi-tenancy

## 📞 Suporte

- 📧 **Email**: arthur.mendes@email.com
- 💬 **Discord**: [PersonalBot Community](https://discord.gg/personalbot)
- 📚 **Wiki**: [Documentação Completa](https://github.com/arthurdev1998/Lab_Mcp/wiki)
- 🐛 **Issues**: [Reportar Bug](https://github.com/arthurdev1998/Lab_Mcp/issues)

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo [LICENSE](LICENSE) para detalhes.

---

**🎯 Status Atual**: Em Desenvolvimento  
**🚀 Última Atualização**: 11/07/2025  
**👨‍💻 Desenvolvedor**: Arthur Mendes  
**🤖 IA Partner**: GitHub Copilot