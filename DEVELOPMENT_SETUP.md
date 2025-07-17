# RATH Development Setup Guide

## Prerequisites

### System Requirements
- **Node.js**: 16.x or later
- **Yarn**: 1.22.x (preferred package manager)
- **Python**: 3.8+ (for backend services)
- **Docker**: Latest version (for containerized services)

### Recommended Development Environment
- **VS Code** with extensions:
  - TypeScript and JavaScript Language Features
  - ESLint
  - Prettier
  - Python
  - Docker

## Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/Kanaries/Rath.git
cd Rath
```

### 2. Install Dependencies
```bash
# Install all workspace dependencies
yarn install

# If you encounter peer dependency issues, use:
yarn install --legacy-peer-deps
# or
npm install --legacy-peer-deps
```

### 3. Build the Project
```bash
# Build all packages in correct order
yarn build

# Or build specific components:
yarn build:utils
yarn build:scenegraph
yarn build:renderer
yarn build:client
```

### 4. Start Development Server
```bash
# Start frontend development server
yarn devfront
# This will start the React app on http://localhost:3000

# In another terminal, start backend services if needed
yarn devback
```

## Project Structure Deep Dive

### Frontend Architecture (`packages/rath-client/`)

```
src/
├── components/          # Reusable UI components
│   ├── appNav/         # Application navigation
│   ├── crInfo/         # Component registration info
│   └── performance-window/ # Performance monitoring
├── pages/              # Main application pages
│   ├── dataSource/     # Data source management
│   ├── dataConnection/ # Database connections
│   ├── megaAutomation/ # AutoPilot features
│   ├── semiAutomation/ # Copilot features
│   ├── manualControl/  # Manual exploration
│   ├── painter/        # Data painter
│   ├── dashboard/      # Dashboard builder
│   ├── causal/         # Causal analysis
│   └── collection/     # Saved insights
├── store/              # MobX state management
├── services/           # Frontend services
├── utils/              # Utility functions
├── workers/            # Web Workers
└── locales.ts          # Internationalization
```

### Key State Management Stores

1. **commonStore.ts**: Global application state
2. **dataSourceStore.ts**: Data loading and management
3. **megaAutomation.ts**: AutoPilot analysis state
4. **painterStore.ts**: Data painter interactions
5. **causalStore/**: Causal analysis workflows
6. **dashboardStore.ts**: Dashboard building state

### Backend Services

Each service is independently deployable:

```
services/
├── causal-service/     # Python FastAPI service
│   ├── algorithms/     # Causal discovery algorithms
│   ├── interfaces.py   # API interfaces
│   ├── main.py        # FastAPI application
│   └── requirements.txt
├── connector/         # Data source connectors
├── narrative-service/ # Natural language generation
├── prediction/        # ML prediction models
└── text-pattern-service/ # Text analysis
```

## Development Workflows

### Frontend Development

1. **Component Development**
   ```bash
   # Start development server with hot reloading
   cd packages/rath-client
   yarn start
   ```

2. **State Management**
   - Use MobX observables for reactive state
   - Follow the existing store patterns
   - Use `useGlobalStore()` hook to access stores

3. **Adding New Features**
   - Create components in `src/components/`
   - Add pages in `src/pages/`
   - Register routes in main App component
   - Add store if needed in `src/store/`

### Backend Service Development

1. **Python Services**
   ```bash
   cd services/causal-service
   pip install -r requirements.txt
   python main.py
   ```

2. **Docker Development**
   ```bash
   # Build and run specific service
   cd services/causal-service
   docker build -t rath-causal .
   docker run -p 8000:8000 rath-causal
   ```

### Testing

```bash
# Run frontend tests
cd packages/rath-client
yarn test

# Run with coverage
yarn test --coverage

# Run specific test files
yarn test src/components/AppNav.test.tsx
```

### Code Quality

```bash
# Lint and format code
yarn lint
yarn format

# Type checking
yarn type-check
```

## Environment Configuration

### Frontend Environment Variables

Create `.env` file in `packages/rath-client/`:

```env
# API endpoints
REACT_APP_CAUSAL_SERVICE_URL=http://localhost:8000
REACT_APP_NARRATIVE_SERVICE_URL=http://localhost:8001
REACT_APP_PREDICTION_SERVICE_URL=http://localhost:8002

# Feature flags
REACT_APP_ENABLE_CAUSAL_ANALYSIS=true
REACT_APP_ENABLE_DATA_PAINTER=true

# Analytics
REACT_APP_ANALYTICS_ID=your-analytics-id
```

### Backend Configuration

Each service may have its own configuration. For example, causal service:

```python
# services/causal-service/.env
DEBUG=true
CORS_ORIGINS=http://localhost:3000,https://rath.kanaries.net
MAX_WORKERS=4
```

## Database Integration

### Supported Data Sources

RATH supports connecting to various databases:

```typescript
// Connection configuration example
const connectionConfig = {
  type: 'postgresql', // mysql, clickhouse, etc.
  host: 'localhost',
  port: 5432,
  database: 'your_db',
  username: 'user',
  password: 'password',
  ssl: false
};
```

### Adding New Database Support

1. Extend connector service in `services/connector/`
2. Add connection UI in `packages/rath-client/src/pages/dataConnection/`
3. Update data source store to handle new connection type

## Deployment

### Development Deployment

```bash
# Build all packages
yarn build

# Start production server
yarn ui
```

### Docker Deployment

```bash
# Build frontend container
docker build -f client.dockerfile -t rath-client .

# Use docker-compose for full stack
docker-compose up -d
```

### Production Deployment

1. **Frontend**: Deploy to CDN/static hosting (Vercel, Netlify, etc.)
2. **Backend Services**: Deploy to cloud providers (AWS, GCP, Azure)
3. **Database**: Use managed database services

## Performance Optimization Tips

### Frontend Performance

1. **Code Splitting**: Use React.lazy() for large components
2. **Memoization**: Use React.memo() and useMemo() appropriately
3. **Virtualization**: For large data tables and lists
4. **Web Workers**: For compute-intensive operations

### Visualization Performance

1. **Canvas Rendering**: For large datasets (>10k points)
2. **Data Sampling**: Implement intelligent sampling for previews
3. **Incremental Updates**: Only re-render changed portions
4. **Memory Management**: Clean up chart instances properly

### Backend Performance

1. **Caching**: Implement Redis for frequently accessed data
2. **Async Processing**: Use background tasks for long-running operations
3. **Database Optimization**: Use appropriate indexes and query optimization
4. **Load Balancing**: Scale services horizontally

## Debugging and Troubleshooting

### Common Issues

1. **Memory Issues**
   ```bash
   # Increase Node.js memory limit
   NODE_OPTIONS="--max-old-space-size=8192" yarn start
   ```

2. **Dependency Conflicts**
   ```bash
   # Clear node_modules and reinstall
   yarn clean
   yarn install
   ```

3. **Build Failures**
   ```bash
   # Clean build cache
   yarn clean
   yarn build:utils
   yarn build:scenegraph
   yarn build:renderer
   yarn build:client
   ```

### Development Tools

1. **React DevTools**: For component debugging
2. **Redux DevTools**: For MobX state inspection
3. **Network Tab**: For API debugging
4. **Performance Tab**: For performance profiling

## Contributing Guidelines

1. **Code Style**: Follow existing TypeScript and React patterns
2. **Testing**: Add unit tests for new features
3. **Documentation**: Update relevant documentation
4. **Performance**: Consider performance impact of changes
5. **Accessibility**: Ensure components are accessible

### Git Workflow

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "feat: add your feature description"

# Push and create PR
git push origin feature/your-feature-name
```

## Resources

- [RATH Documentation](https://docs.kanaries.net)
- [Community Discord](https://discord.gg/Z4ngFWXz2U)
- [GitHub Issues](https://github.com/Kanaries/Rath/issues)
- [Contributing Guide](https://docs.kanaries.net/community/contribution-guide)

## Next Steps

After setting up your development environment:

1. Explore the codebase starting with `packages/rath-client/src/App.tsx`
2. Try building a simple feature or fixing a small bug
3. Read through the existing components to understand patterns
4. Join the community discussions for questions and collaboration

Happy coding! 🚀