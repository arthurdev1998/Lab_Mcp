# 🏗️ PLANO TÉCNICO - PersonalBot

> **⚠️ LEITURA OBRIGATÓRIA:** Este documento deve ser lido antes de iniciar qualquer feature ou desenvolvimento.

## 🎯 Arquitetura Geral

### Clean Architecture + DDD

```
┌─────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER                       │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   React SPA     │  │  REST API       │  │   SignalR       │ │
│  │   (Frontend)    │  │  (Controllers)  │  │   (Real-time)   │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────┐
│                       APPLICATION LAYER                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Use Cases     │  │    Commands     │  │    Queries      │ │
│  │   (MediatR)     │  │   (CQRS)        │  │   (CQRS)        │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────┐
│                         DOMAIN LAYER                            │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Entities      │  │  Value Objects  │  │ Domain Services │ │
│  │   (Aggregates)  │  │   (Immutable)   │  │  (Business)     │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
                               │
┌─────────────────────────────────────────────────────────────────┐
│                      INFRASTRUCTURE LAYER                       │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │   Repository    │  │   Llama Client  │  │   External      │ │
│  │   (EF Core)     │  │   (AI Service)  │  │   Services      │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
└─────────────────────────────────────────────────────────────────┘
```

## 📁 Estrutura de Projetos

```
PersonalBot/
├── src/
│   ├── PersonalBot.Domain/                 # 🎯 Regras de Negócio
│   │   ├── Entities/
│   │   ├── ValueObjects/
│   │   ├── Services/
│   │   ├── Repositories/
│   │   └── Events/
│   │
│   ├── PersonalBot.Application/            # 🔄 Casos de Uso
│   │   ├── Commands/
│   │   ├── Queries/
│   │   ├── Handlers/
│   │   ├── DTOs/
│   │   └── Mappers/
│   │
│   ├── PersonalBot.Infrastructure/         # 🔧 Implementações
│   │   ├── Persistence/
│   │   ├── Repositories/
│   │   ├── LlamaService/
│   │   └── ExternalServices/
│   │
│   ├── PersonalBot.WebAPI/                 # 🌐 API REST
│   │   ├── Controllers/
│   │   ├── Middleware/
│   │   ├── Hubs/
│   │   └── Configuration/
│   │
│   └── PersonalBot.UI/                     # ⚛️ React SPA
│       ├── src/
│       │   ├── components/
│       │   ├── pages/
│       │   ├── services/
│       │   ├── store/
│       │   └── types/
│       └── public/
│
├── tests/
│   ├── PersonalBot.Domain.Tests/
│   ├── PersonalBot.Application.Tests/
│   ├── PersonalBot.Infrastructure.Tests/
│   ├── PersonalBot.WebAPI.Tests/
│   └── PersonalBot.IntegrationTests/
│
└── docs/
    ├── api/
    ├── architecture/
    └── deployment/
```

## 🏛️ Domain Driven Design (DDD)

### Bounded Context: Produtividade Pessoal

#### Aggregates
1. **User Aggregate**
   - User (Root)
   - UserPreferences
   - UserSettings

2. **Task Aggregate**
   - Task (Root)
   - SubTask
   - TaskComment
   - TaskAttachment

3. **Conversation Aggregate**
   - Conversation (Root)
   - Message
   - MessageContext

#### Value Objects
```csharp
public record TaskPriority(int Value)
{
    public static TaskPriority Low => new(1);
    public static TaskPriority Medium => new(2);
    public static TaskPriority High => new(3);
    public static TaskPriority Critical => new(4);
}

public record Email(string Value)
{
    public Email
    {
        if (!IsValid(Value))
            throw new ArgumentException("Invalid email format");
    }
}
```

#### Domain Services
```csharp
public interface ITaskSchedulingService
{
    Task<ScheduleResult> OptimizeSchedule(UserId userId, IEnumerable<Task> tasks);
}

public interface IProductivityAnalysisService
{
    Task<ProductivityMetrics> AnalyzeUserProductivity(UserId userId, DateRange period);
}
```

## 🎯 SOLID Principles Implementation

### Single Responsibility Principle (SRP)
```csharp
// ✅ Cada classe tem uma única responsabilidade
public class TaskCreationHandler : IRequestHandler<CreateTaskCommand, TaskDto>
public class TaskValidationService : ITaskValidationService
public class TaskNotificationService : ITaskNotificationService
```

### Open/Closed Principle (OCP)
```csharp
// ✅ Extensível via interfaces
public interface ILlamaClientStrategy
{
    Task<string> GenerateResponseAsync(string prompt);
}

public class LocalLlamaClient : ILlamaClientStrategy { }
public class CloudLlamaClient : ILlamaClientStrategy { }
```

### Liskov Substitution Principle (LSP)
```csharp
// ✅ Implementações substituíveis
public abstract class BaseRepository<T> where T : IAggregateRoot
{
    public virtual async Task<T> GetByIdAsync(Guid id) { }
}

public class TaskRepository : BaseRepository<Task>, ITaskRepository { }
```

### Interface Segregation Principle (ISP)
```csharp
// ✅ Interfaces específicas
public interface ITaskReader
{
    Task<Task> GetByIdAsync(Guid id);
}

public interface ITaskWriter
{
    Task AddAsync(Task task);
    Task UpdateAsync(Task task);
}
```

### Dependency Inversion Principle (DIP)
```csharp
// ✅ Dependências invertidas
public class TaskService
{
    private readonly ITaskRepository _taskRepository;
    private readonly ILlamaClientStrategy _llamaClient;
    
    public TaskService(ITaskRepository taskRepository, ILlamaClientStrategy llamaClient)
    {
        _taskRepository = taskRepository;
        _llamaClient = llamaClient;
    }
}
```

## 🔄 CQRS + MediatR Pattern

### Commands (Write Operations)
```csharp
public record CreateTaskCommand(
    string Title,
    string Description,
    TaskPriority Priority,
    DateTime? DueDate,
    Guid CategoryId
) : IRequest<TaskDto>;

public class CreateTaskHandler : IRequestHandler<CreateTaskCommand, TaskDto>
{
    public async Task<TaskDto> Handle(CreateTaskCommand request, CancellationToken cancellationToken)
    {
        // Validation
        // Business Logic
        // Persistence
        // Event Publishing
    }
}
```

### Queries (Read Operations)
```csharp
public record GetUserTasksQuery(
    Guid UserId,
    TaskStatus? Status = null,
    TaskPriority? Priority = null,
    int Page = 1,
    int PageSize = 20
) : IRequest<PagedResult<TaskDto>>;

public class GetUserTasksHandler : IRequestHandler<GetUserTasksQuery, PagedResult<TaskDto>>
{
    // Read-only operations
}
```

## 🤖 Integração com Llama Local

### Llama Service Architecture
```csharp
public interface ILlamaService
{
    Task<LlamaResponse> GenerateResponseAsync(LlamaRequest request);
    Task<TaskSuggestion[]> SuggestTasksAsync(string userInput);
    Task<string> AnalyzeProductivityAsync(ProductivityData data);
}

public class LlamaService : ILlamaService
{
    private readonly ILlamaClientStrategy _client;
    private readonly IPromptEngineering _promptService;
    private readonly IContextManager _contextManager;
    
    public async Task<LlamaResponse> GenerateResponseAsync(LlamaRequest request)
    {
        var context = await _contextManager.BuildContextAsync(request.UserId);
        var prompt = _promptService.BuildPrompt(request.Message, context);
        return await _client.GenerateResponseAsync(prompt);
    }
}
```

### Prompt Engineering
```csharp
public class PromptEngineering : IPromptEngineering
{
    public string BuildTaskCreationPrompt(string userInput, UserContext context)
    {
        return $@"
You are a productivity assistant. Based on the user input: '{userInput}'
User context: {JsonSerializer.Serialize(context)}

Extract task information and respond in JSON format:
{{
    ""title"": ""task title"",
    ""description"": ""task description"",
    ""priority"": ""low|medium|high|critical"",
    ""dueDate"": ""ISO date or null"",
    ""category"": ""category name""
}}
";
    }
}
```

## 🗄️ Data Layer (Entity Framework)

### Entities Configuration
```csharp
public class TaskConfiguration : IEntityTypeConfiguration<Task>
{
    public void Configure(EntityTypeBuilder<Task> builder)
    {
        builder.HasKey(t => t.Id);
        
        builder.Property(t => t.Title)
               .IsRequired()
               .HasMaxLength(200);
               
        builder.OwnsOne(t => t.Priority, p =>
        {
            p.Property(x => x.Value).HasColumnName("Priority");
        });
        
        builder.HasMany(t => t.SubTasks)
               .WithOne()
               .HasForeignKey("TaskId")
               .OnDelete(DeleteBehavior.Cascade);
    }
}
```

### Repository Pattern
```csharp
public class TaskRepository : ITaskRepository
{
    private readonly PersonalBotDbContext _context;
    
    public async Task<Task> GetByIdAsync(Guid id)
    {
        return await _context.Tasks
            .Include(t => t.SubTasks)
            .Include(t => t.Category)
            .FirstOrDefaultAsync(t => t.Id == id);
    }
    
    public async Task<PagedResult<Task>> GetUserTasksAsync(GetUserTasksQuery query)
    {
        var queryable = _context.Tasks
            .Where(t => t.UserId == query.UserId);
            
        if (query.Status.HasValue)
            queryable = queryable.Where(t => t.Status == query.Status);
            
        return await queryable.ToPagedResultAsync(query.Page, query.PageSize);
    }
}
```

## 🌐 API Design

### RESTful Endpoints
```csharp
[ApiController]
[Route("api/[controller]")]
public class TasksController : ControllerBase
{
    private readonly IMediator _mediator;
    
    [HttpPost]
    public async Task<ActionResult<TaskDto>> CreateTask([FromBody] CreateTaskCommand command)
    {
        var result = await _mediator.Send(command);
        return CreatedAtAction(nameof(GetTask), new { id = result.Id }, result);
    }
    
    [HttpGet("{id:guid}")]
    public async Task<ActionResult<TaskDto>> GetTask(Guid id)
    {
        var query = new GetTaskByIdQuery(id);
        var result = await _mediator.Send(query);
        return result != null ? Ok(result) : NotFound();
    }
}
```

### Chat Controller
```csharp
[ApiController]
[Route("api/[controller]")]
public class ChatController : ControllerBase
{
    [HttpPost("message")]
    public async Task<ActionResult<ChatResponse>> SendMessage([FromBody] SendMessageCommand command)
    {
        var response = await _mediator.Send(command);
        return Ok(response);
    }
}
```

## ⚛️ Frontend Architecture (React)

### State Management (Redux Toolkit)
```typescript
// Store structure
interface AppState {
  auth: AuthState;
  tasks: TaskState;
  chat: ChatState;
  ui: UIState;
}

// Task slice
const taskSlice = createSlice({
  name: 'tasks',
  initialState,
  reducers: {
    addTask: (state, action) => {
      state.tasks.push(action.payload);
    },
    updateTask: (state, action) => {
      const index = state.tasks.findIndex(t => t.id === action.payload.id);
      if (index !== -1) {
        state.tasks[index] = action.payload;
      }
    }
  }
});
```

### Component Architecture
```typescript
// Smart Component (Container)
const TaskListContainer: React.FC = () => {
  const tasks = useAppSelector(selectTasks);
  const dispatch = useAppDispatch();
  
  const handleCreateTask = useCallback((taskData: CreateTaskRequest) => {
    dispatch(createTaskAsync(taskData));
  }, [dispatch]);
  
  return <TaskList tasks={tasks} onCreateTask={handleCreateTask} />;
};

// Dumb Component (Presentation)
interface TaskListProps {
  tasks: Task[];
  onCreateTask: (task: CreateTaskRequest) => void;
}

const TaskList: React.FC<TaskListProps> = ({ tasks, onCreateTask }) => {
  return (
    <div>
      {tasks.map(task => (
        <TaskItem key={task.id} task={task} />
      ))}
    </div>
  );
};
```

## 🧪 Testing Strategy

### Unit Tests
```csharp
public class CreateTaskHandlerTests
{
    [Fact]
    public async Task Handle_ValidCommand_ShouldCreateTask()
    {
        // Arrange
        var command = new CreateTaskCommand("Test Task", "Description", TaskPriority.Medium, null, Guid.NewGuid());
        var mockRepository = new Mock<ITaskRepository>();
        var handler = new CreateTaskHandler(mockRepository.Object);
        
        // Act
        var result = await handler.Handle(command, CancellationToken.None);
        
        // Assert
        Assert.NotNull(result);
        Assert.Equal(command.Title, result.Title);
        mockRepository.Verify(r => r.AddAsync(It.IsAny<Task>()), Times.Once);
    }
}
```

### Integration Tests
```csharp
public class TasksControllerIntegrationTests : IClassFixture<WebApplicationFactory<Program>>
{
    [Fact]
    public async Task CreateTask_ValidData_ReturnsCreatedTask()
    {
        // Arrange
        var client = _factory.CreateClient();
        var command = new CreateTaskCommand("Integration Test", "Description", TaskPriority.High, null, Guid.NewGuid());
        
        // Act
        var response = await client.PostAsJsonAsync("/api/tasks", command);
        
        // Assert
        response.EnsureSuccessStatusCode();
        var task = await response.Content.ReadFromJsonAsync<TaskDto>();
        Assert.Equal(command.Title, task.Title);
    }
}
```

## 📊 Monitoring e Observability

### Structured Logging (Serilog)
```csharp
public class CreateTaskHandler : IRequestHandler<CreateTaskCommand, TaskDto>
{
    private readonly ILogger<CreateTaskHandler> _logger;
    
    public async Task<TaskDto> Handle(CreateTaskCommand request, CancellationToken cancellationToken)
    {
        _logger.LogInformation("Creating task for user {UserId} with title {Title}", 
            request.UserId, request.Title);
            
        try
        {
            // Implementation
            _logger.LogInformation("Task {TaskId} created successfully", result.Id);
            return result;
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error creating task for user {UserId}", request.UserId);
            throw;
        }
    }
}
```

### Health Checks
```csharp
services.AddHealthChecks()
    .AddDbContext<PersonalBotDbContext>()
    .AddCheck<LlamaServiceHealthCheck>("llama-service")
    .AddCheck<DatabaseHealthCheck>("database");
```

## 🔒 Security Guidelines

### Authentication & Authorization
```csharp
[Authorize]
[ApiController]
public class TasksController : ControllerBase
{
    [HttpGet]
    [RequirePermission("tasks:read")]
    public async Task<ActionResult<IEnumerable<TaskDto>>> GetTasks()
    {
        var userId = User.GetUserId();
        var query = new GetUserTasksQuery(userId);
        return Ok(await _mediator.Send(query));
    }
}
```

### Input Validation
```csharp
public class CreateTaskCommandValidator : AbstractValidator<CreateTaskCommand>
{
    public CreateTaskCommandValidator()
    {
        RuleFor(x => x.Title)
            .NotEmpty()
            .MaximumLength(200);
            
        RuleFor(x => x.Description)
            .MaximumLength(2000);
            
        RuleFor(x => x.DueDate)
            .GreaterThan(DateTime.UtcNow)
            .When(x => x.DueDate.HasValue);
    }
}
```

## 📋 Checklist Pré-Desenvolvimento

Antes de iniciar qualquer feature:

### 🎯 Planejamento
- [ ] Li e entendi o Plano de Ação
- [ ] Li e entendi o Plano Técnico
- [ ] Identifiquei o bounded context da feature
- [ ] Defini os agregados envolvidos
- [ ] Identifiquei commands/queries necessários

### 🏗️ Arquitetura
- [ ] Feature segue Clean Architecture
- [ ] Aplica princípios SOLID
- [ ] Utiliza DDD adequadamente
- [ ] Implementa CQRS se necessário
- [ ] Segue padrões estabelecidos

### ✅ Qualidade
- [ ] Testes unitários planejados
- [ ] Validações implementadas
- [ ] Logs estruturados adicionados
- [ ] Error handling adequado
- [ ] Performance considerations

### 📝 Documentação
- [ ] API documentada (Swagger)
- [ ] README atualizado se necessário
- [ ] Comentários em código complexo
- [ ] Migrations documentadas

---

**🎯 LEMBRE-SE:** Este documento é a bíblia técnica do projeto. Consulte sempre antes de implementar qualquer feature!

**📞 Em caso de dúvidas:** Revise os princípios DDD, Clean Architecture e SOLID antes de prosseguir.
