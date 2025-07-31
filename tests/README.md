# 📊 Test Reports and Coverage - Eulalia Backend

This folder contains all artifacts related to unit and integration testing of the **Eulalia Backend** project.

---

## 🧪 Test Execution Summary

- **Total tests executed**: 130  
- **Passed**: ✅ 130  
- **Failed**: ❌ 0  
- **Skipped**: 0  
- **Execution time**: ~2.3 seconds  
- **Test framework**: xUnit  
- **Assertion library**: FluentAssertions  

---

## ⚙️ Commands Used

### 1. Execute Tests with Coverage (Cobertura XML output)
```bash
dotnet test ^
  /p:CollectCoverage=true ^
  /p:CoverletOutput=./TestResults/coverage ^
  /p:CoverletOutputFormat=cobertura
```

### 2. Alternatively, using `coverlet.console`:
```bash
coverlet bin\Debug\net8.0\eulalia-backend.Tests.dll ^
  --target "dotnet" ^
  --targetargs "test --no-build" ^
  --format cobertura ^
  --output TestResults\coverage
```

### 3. Generate HTML report with `reportgenerator`
```bash
reportgenerator ^
  -reports:"TestResults\coverage.cobertura.xml" ^
  -targetdir:"coverage-report" ^
  -reporttypes:Html
```

---

## 📁 Folder Structure

```
tests/
├── coverage-report/          # HTML coverage report (index.html)
└── TestResults/              # Raw test results and cobertura file
```

---

## 📈 Coverage Summary

| Module                         | Line     | Branch   | Method   |
|-------------------------------|----------|----------|----------|
| eulalia-backend.Api           | 23.31 %  | 13.63 %  | 14.86 %  |
| eulalia-backend.Application   | 100 %    | 100 %    | 100 %    |
| eulalia-backend.Domain        | 57.69 %  | 100 %    | 57.69 %  |
| eulalia-backend.Infrastructure | 3.66 %  | 87.5 %   | 77.77 %  |
| eulalia-backend.Shared        | 100 %    | 100 %    | 100 %    |
| **Total Average**             | **12.48 %** | **25 %** | **41.17 %** |

---

## 📦 Tools Used

- [`xUnit`](https://xunit.net/)
- [`FluentAssertions`](https://fluentassertions.com/)
- [`coverlet.console`](https://github.com/coverlet-coverage/coverlet)
- [`reportgenerator`](https://danielpalme.github.io/ReportGenerator/)

---

## 📝 Notes

- This report was generated as part of **Milestone 2**.
- Code coverage is expected to improve in future iterations.
- Reports are automatically generated to support testing transparency.