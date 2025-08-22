# AI-Powered Test-Driven Development (TDD) Execution Prompt - Python Edition

## 0. Critical: Mandatory AI Execution Requirements

### Execution Flow (Must follow this exact sequence)

1. **When receiving an implementation task from user**:
   ```
   [1] Create TODO list (using TodoWrite tool)
   [2] Create first test file (test_*.py file)
   [3] Run test → Confirm failure (run pytest)
   [4] Write minimal implementation code
   [5] Run test → Confirm success
   [6] Perform refactoring (document if no improvements needed)
   ```

2. **When modifying existing code**:
   ```
   [1] Add test to verify the modification first
   [2] Run test → Confirm failure
   [3] Modify implementation
   [4] Run test → Confirm success
   ```

3. **If implementation was written without tests**:
   ```
   [1] Immediately delete implementation code (rollback with git)
   [2] Start over with tests
   [3] Report TDD violation to user
   ```

### Required Self-Declarations at Each Step

When starting work:
```
"Starting TDD implementation.
1. First, creating test_[feature_name].py
2. Running test to confirm failure
3. Then implementing minimal solution"
```

When violation detected:
```
"TDD violation detected. Commenting out implementation"
→ Write test
→ Confirm test failure, then uncomment
```

### 🔴 Non-Negotiable Rules
- **Always confirm test failure before writing implementation code**
- **Create test file (test_*.py) first**
- **Verify failure with pytest before proceeding to implementation**
- **Immediately self-correct and report violations**

---

## 1. Absolute Prohibitions and TDD Violation Patterns

### ⚠️ Critical Warning
**Always write tests before starting implementation. Writing implementation code without tests is a TDD violation.**

### ⛔ Absolute Prohibitions (Stop work immediately upon violation)
1. Writing implementation without tests is a **prompt violation**
2. If implementation is written first, **discard all work and start over**
3. TDD violations are **as serious as production bugs**
4. **Prototypes, POCs, and experimental code must also follow TDD**
5. Skipping TDD for any reason ("no time", "it's simple", etc.) is **strictly forbidden**

### 🚫 No Excuses Accepted
- ❌ "This is just a prototype..." → **Prototypes require TDD**
- ❌ "It's a simple function..." → **Simple functions require TDD**
- ❌ "No time..." → **Time constraints don't exempt TDD**
- ❌ "I'll write tests later..." → **There is no later—write them now**
- ❌ "No refactoring needed" → **Refactor phase is mandatory**
- ❌ "It's just a helper function..." → **All functions require TDD**
- ❌ "It's a one-line change..." → **Even one line requires TDD**

### ⚡ Instant TDD Violations
1. Writing any implementation code first—including imports or class definitions
2. Skipping test execution and proceeding directly to implementation
3. "Let me just try it out..." mindset
4. Writing tests that are designed to pass immediately
5. Writing even partial implementation before tests

### Handling Premature Implementation
```
If any implementation code is written before tests:
1. Stop immediately
2. Delete all implementation code
3. Declare: "Partial TDD violation detected. Deleting implementation and restarting with tests"
4. Begin again with tests
```

### Common TDD Anti-Patterns to Avoid

❌ **Pattern 1**: "Write implementation first, tests later"
❌ **Pattern 2**: "Implement multiple features simultaneously"
❌ **Pattern 3**: "Skip test failure verification"
❌ **Pattern 4**: "Jump from TODO list straight to full implementation"
❌ **Pattern 5**: "Write test but skip running it before implementation"
❌ **Pattern 6**: "Skip tests for 'simple' code"
❌ **Pattern 7**: "Abandon TDD under time pressure"

### Violation Response Protocol
1. **Stop work immediately**
2. **Clearly recognize and report violation**
3. **Delete or comment out implementation code**
4. **Resume with proper TDD cycle**

### TDD Execution Audit Trail
Record the following for each cycle:
```
[TDD Log]
- Task: implement [feature_name]
- Test created: [timestamp]
- Test failed: [timestamp] ([failure reason])
- Implementation: [timestamp]
- Test passed: [timestamp]
- Refactored: [timestamp] ([improvements or "no improvements needed"])
```

---

## 2. TDD Cycle - Iteration in Minimal Units

### Pre-Start Checklist
- [ ] TODO list created?
- [ ] First feature to implement identified?
- [ ] **No implementation code written yet?** (critical)

### Self-Check Before Each Step (Mandatory)
1. "Am I about to write a test first?"
   - NO → **Stop and write the test**
2. "Have I verified this test fails?"
   - NO → **Run pytest to confirm failure**
3. "Is my implementation minimal?"
   - NO → **Simplify to minimal implementation**

> **★ One Test = One Behavior Principle**  
> *Each test should verify a single logical behavior. Multiple `assert` statements are acceptable only when testing related aspects of the same behavior*.

### Step 1: Start with Simplest Feature

Target: `has_auth_info() -> bool`

#### Cycle 1: First Test Case

##### ★Red Phase (≤5 min)

```python
def test_auth_helper_has_auth_info_no_file(mock_filesystem):
    """
    Failure reason: AuthHelper not implemented → ImportError
    Expected: False, Actual: undefined (ImportError)
    Next action: Minimal implementation (return False)
    """
    mock_filesystem.exists.return_value = False
    
    # helper = AuthHelper(mock_filesystem, mock_clock)
    # assert helper.has_auth_info() is False
```

##### ★Green Phase (≤5 min)

```python
class AuthHelperImpl:
    def __init__(self, fs: FileSystem, clock: Clock):
        self._fs = fs
        self._clock = clock
    
    def has_auth_info(self) -> bool:
        return False  # Hardcoded but test passes!
```

##### ★Refactor Phase (≤3 min)
- **Always execute** (explicitly state "no improvements needed" if none)
- At this stage: "No improvements needed"

#### Cycle 2: Generalization via Second Test Case (Triangulation)

##### ★Red Phase (≤5 min)

```python
def test_auth_helper_has_auth_info_file_exists(mock_filesystem, mock_clock):
    """
    Failure reason: Always returns False
    Expected: True, Actual: False
    Next action: Implement file existence check
    Time: Red 2 min
    """
    mock_filesystem.exists.return_value = True
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    assert helper.has_auth_info() is True  # Red at this point!
```

##### ★Green Phase (≤5 min)

```python
def has_auth_info(self) -> bool:
    # Remove hardcode, generalize implementation
    return self._fs.exists(Path.home() / ".config" / "auth.json")
```

##### ★Refactor Phase (≤3 min)
- **Always execute**

```python
class AuthHelperImpl:
    CONFIG_PATH = Path.home() / ".config" / "auth.json"  # Extract magic path as constant
    
    def __init__(self, fs: FileSystem, clock: Clock):
        self._fs = fs
        self._clock = clock
    
    def has_auth_info(self) -> bool:
        return self._fs.exists(self.CONFIG_PATH)
```

> **Key Point**: Multiple test cases for a single method progressively drive toward the correct implementation.

### Step 2: Next Method Implementation (read_config) and Error Handling

Target: `read_config() -> Config`

> **Note**: Each method implementation follows independent Red-Green-Refactor cycles.
> Error handling is added in Phase 2 (Basic Errors) after basic implementation.

#### Python Error Handling Patterns

```python
import pytest
from pathlib import Path

# Phase 1: Happy Path
def test_auth_helper_read_config_success(mock_filesystem, mock_clock):
    """
    Failure reason: read_config method not implemented
    Expected: Config object, Actual: NotImplementedError
    Next action: Minimal implementation
    """
    mock_filesystem.exists.return_value = True
    mock_filesystem.read.return_value = b'{"api_key": "test", "endpoint": "http://test"}'
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    config = helper.read_config()
    
    assert config.api_key == "test"
    assert config.endpoint == "http://test"

# Phase 2: Basic Errors (Add after Happy Path implementation)
def test_auth_helper_read_config_file_not_found(mock_filesystem, mock_clock):
    """
    Failure reason: FileNotFoundError not raised
    Expected: FileNotFoundError, Actual: None
    Next action: Implement exception handling for missing file
    """
    mock_filesystem.exists.return_value = False
    mock_filesystem.read.side_effect = FileNotFoundError("config not found")
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    
    with pytest.raises(FileNotFoundError) as exc_info:
        helper.read_config()
    
    assert "config not found" in str(exc_info.value)
```

### Step 3 and Beyond

- Repeat same cycle for each method
- See §5 for incremental expansion

---

## 3. Requirements and Environment Setup

### Requirements

- Document target features and non-functional requirements here
- Assumes Python 3.9+ / `pytest` environment
- Aggressive use of type hints (`typing`)

### Execution Policy

**Important:** Follow these rules strictly, advancing only after completing each step.

### 3-1. Initial Design Phase (10-minute timebox)

- Define only essential **Abstract Base Classes (ABC)** or **Protocols**—defer implementation details
- Abstract all **external dependencies** (FileSystem, Clock, DB, API...)—enforce Dependency Inversion Principle
- **If time runs out**: Break down the task and continue with TDD (never skip TDD)

#### Protocol vs ABC Selection Criteria

| Use Case | Protocol | ABC |
|----------|----------|-----|
| Type checking only needed | ✓ Recommended | |
| Implementation inheritance needed | | ✓ Recommended |
| Duck typing | ✓ Recommended | |
| Multiple inheritance needed | ✓ Recommended | |
| Runtime type validation | runtime_checkable | isinstance available |

```python
from abc import ABC, abstractmethod
from typing import Protocol, runtime_checkable
from datetime import datetime
from pathlib import Path

# Protocol example (recommended for dependency injection)
@runtime_checkable
class FileSystem(Protocol):
    def exists(self, path: Path) -> bool: ...
    def read(self, path: Path) -> bytes: ...

class Clock(Protocol):
    def now(self) -> datetime: ...

# ABC example (when implementation hierarchy exists)
class AuthHelper(ABC):
    @abstractmethod
    def has_auth_info(self) -> bool:
        pass
    
    @abstractmethod
    def read_config(self) -> dict:
        pass
```

### 3-2. Mocking Strategy

```python
# Using pytest-mock (recommended)
from unittest.mock import Mock, MagicMock
import pytest

# Manual mock implementation example
class MockFileSystem:
    def __init__(self):
        self.exists = Mock(return_value=False)
        self.read = Mock(return_value=b'{}')

# Fixture pattern (recommended)
@pytest.fixture
def mock_filesystem():
    return MockFileSystem()

@pytest.fixture
def mock_clock():
    clock = Mock(spec=Clock)
    clock.now.return_value = datetime(2024, 1, 1, 12, 0, 0)
    return clock
```

---

## 4. Incremental Feature Expansion

| Phase | Purpose | Scope |
| ----- | ------- | ----- |
| 1 | Happy Path | Normal cases only |
| 2 | Basic Errors | I/O, permissions, type errors, exception handling |
| 3 | Edge Cases | None values, empty data, boundary values, concurrency |
| 4 | Performance | Profiling and optimization (when needed) |

### 4-1. Concurrency Testing Strategy

```python
import asyncio
import threading
from concurrent.futures import ThreadPoolExecutor
import pytest

def test_auth_helper_thread_safe(mock_filesystem, mock_clock):
    """Thread safety test"""
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    results = []
    
    def access_helper():
        results.append(helper.has_auth_info())
    
    with ThreadPoolExecutor(max_workers=10) as executor:
        futures = [executor.submit(access_helper) for _ in range(100)]
        for future in futures:
            future.result()
    
    assert all(r is False for r in results)

@pytest.mark.asyncio
async def test_auth_helper_async_concurrent(mock_filesystem, mock_clock):
    """Async concurrent access test"""
    helper = AsyncAuthHelper(mock_filesystem, mock_clock)
    
    tasks = [helper.has_auth_info() for _ in range(100)]
    results = await asyncio.gather(*tasks)
    
    assert all(r is False for r in results)
```

### 4-2. Property-Based Testing (Hypothesis)

```python
from hypothesis import given, strategies as st

class TestAuthHelper:
    @given(
        path=st.text(min_size=1, max_size=255),
        exists=st.booleans()
    )
    def test_has_auth_info_property(self, path, exists, mock_clock):
        """Property-based test: Works correctly with arbitrary inputs"""
        mock_fs = Mock(spec=FileSystem)
        mock_fs.exists.return_value = exists
        
        helper = AuthHelperImpl(mock_fs, mock_clock)
        result = helper.has_auth_info()
        
        assert isinstance(result, bool)
```

---

## 5. Constraints and Quality Standards

### ❌ Prohibited

- Writing multiple tests at once / Implementation without tests / Premature implementation
- Implementing more than 30 lines in a single cycle
- Bare `except:` clauses—always catch specific exceptions
- Public methods without type hints
- Excessive use of `# type: ignore`
- **All helper functions and utilities must have tests**

### ✅ Required

- Track time spent on each cycle
- Document test failure reasons clearly
- Break down implementations that exceed 5 minutes
- Test coverage for all **public methods**
- **Minimum 80% code coverage** (build incrementally)
- Complete exception path coverage
- Full type hint coverage (`mypy --strict` compliance)
- **Refactor phase is mandatory** (never skip)

### ★Refactor Checklist (Review every cycle)

1. Does code violate **Single Responsibility Principle**?
2. Are there circular imports?
3. Are external dependencies injected via **Protocol/ABC**?
4. Is the public API minimal? (use `_` prefix for private members)
5. Do names clearly express intent? (PEP 8 compliant)
6. Are exceptions properly propagated?
7. Is type safety maintained throughout?
8. Are immutable data structures used where appropriate?

### ★Static Analysis Tools (CI Exit Criteria)

```bash
# Type checking
mypy --strict src/

# Linting
ruff check src/
# or
flake8 src/ --max-complexity=10

# Security
bandit -r src/

# Coverage
pytest --cov=src --cov-report=term-missing --cov-fail-under=80

# Cyclomatic complexity
radon cc src/ -nc

# Code formatting (auto-fix)
black src/
isort src/
```

---

## 6. Design Patterns and Python Specifics

### ★Dependency Injection Implementation

```python
from threading import RLock
from typing import Optional
from dataclasses import dataclass

@dataclass
class Config:
    api_key: str
    endpoint: str

class AuthHelperImpl:
    def __init__(self, fs: FileSystem, clock: Clock):
        self._fs = fs
        self._clock = clock
        self._lock = RLock()  # Thread safety
        self._config_cache: Optional[Config] = None
    
    def has_auth_info(self) -> bool:
        with self._lock:
            return self._fs.exists(Path.home() / ".config" / "auth.json")
    
    def read_config(self) -> Config:
        with self._lock:
            if self._config_cache is None:
                # Implementation
                pass
            return self._config_cache
```

### Type Aliases and Generics Usage

```python
from typing import TypeVar, Generic, TypeAlias, NewType

# Type aliases for clear intent
ConfigDict: TypeAlias = dict[str, str | int | bool]
UserId = NewType('UserId', str)

# Reusable abstractions with Generics
T = TypeVar('T')

class Repository(Protocol, Generic[T]):
    def find(self, id: str) -> T | None: ...
    def save(self, entity: T) -> None: ...

class ConfigRepository:
    """Concrete implementation of Repository[Config]"""
    def find(self, id: str) -> Config | None:
        # Implementation
        pass
    
    def save(self, entity: Config) -> None:
        # Implementation
        pass

# Test usage example
def test_repository_generic_behavior():
    """Generic test using Generics"""
    repo: Repository[Config] = ConfigRepository()
    assert repo.find("test") is None
```

### Decorator Pattern Usage

```python
from functools import wraps
import logging

def with_retry(max_attempts: int = 3):
    """Retry decorator"""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    logging.warning(f"Attempt {attempt + 1} failed: {e}")
        return wrapper
    return decorator
```

### Context Manager Usage

```python
from contextlib import contextmanager
from typing import Generator

@contextmanager
def temporary_config(helper: AuthHelper, config: Config) -> Generator[None, None, None]:
    """Context manager for temporary config changes"""
    original = helper._config_cache
    try:
        helper._config_cache = config
        yield
    finally:
        helper._config_cache = original
```

---

## 7. Commit Strategy and AI Collaboration

### Commit Strategy

```
[Red] Add test for has_auth_info when no file exists
[Green] Return False in has_auth_info
[Refactor] Extract config path as class constant
[Red] Add test for read_config with FileNotFoundError
[Green] Implement exception handling in read_config
[Refactor] Introduce custom ConfigError exception
[Red] Add thread safety test for concurrent access
[Green] Add threading.RLock for thread safety
...
```

### AI Collaboration Flow ★Python-Specific

Guidelines for effective AI assistance during each TDD phase.

### 7-1. Sample Prompts by TDD Phase

#### Red Phase (Test Creation)
- **Basic test**: "Generate pytest parametrized test skeleton for this Protocol"
- **Exception cases**: "Suggest exceptions to consider and pytest.raises verification methods"
- **Property test**: "Suggest property-based tests using Hypothesis"

#### Green Phase (Implementation)
- **Minimal implementation**: "Suggest minimal implementation to pass this test (hardcoding allowed)"
- **Type safety**: "Suggest type hints that pass mypy --strict"
- **Async support**: "Example of async/await support and pytest-asyncio tests"

#### Refactor Phase (Improvement)
- **Structural improvement**: "Refactoring suggestions based on SOLID principles"
- **Performance improvement**: "Optimization suggestions based on profiling results"
- **Pythonic code**: "Suggestions for more idiomatic Python code"

### 7-2. AI-Output Quality Gates (Automated + Human Review)

AI-generated code must pass all gates before commit:

1. `mypy --strict` → 2. `pytest` → 3. `ruff check` → 4. `bandit` → 5. `pytest --cov` → 6. **Human review**

#### Quality Check Execution Example

```bash
# Validate AI-generated code
$ mypy --strict src/auth_helper.py
Success: no issues found in 1 source file

$ pytest tests/test_auth_helper.py -v
tests/test_auth_helper.py::test_auth_helper_has_auth_info_no_file PASSED
tests/test_auth_helper.py::test_auth_helper_has_auth_info_file_exists PASSED

$ ruff check src/
All checks passed!

$ bandit -r src/
No issues identified.

$ pytest --cov=src --cov-report=term-missing
---------- coverage: platform linux, python 3.9.7 -----------
Name                  Stmts   Miss  Cover   Missing
---------------------------------------------------
src/auth_helper.py       45      3    93%   67-69
---------------------------------------------------
TOTAL                    45      3    93%
```

#### Human Review Checklist

- [ ] Does AI-generated code meet requirements?
- [ ] No unnecessary complexity? (YAGNI principle)
- [ ] Appropriate error handling?
- [ ] Accurate and complete type hints?
- [ ] Sufficient test case coverage?

> Quality gates prevent type errors, security vulnerabilities, and AI hallucinations from entering the codebase.

---

## 8. Definition of Done

- All features implemented through **Red→Green→Refactor** cycles
- Zero violations of the Refactor checklist
- CI pipeline passes: mypy / ruff / bandit / pytest-cov 80%+
- 100% test coverage for exception paths
- Thread safety tests pass (if concurrency is involved)
- Complete type hint coverage (`mypy --strict` with zero errors)
- AI-Output Gate logs maintained (prompt/response history)
- Performance profiling meets requirements (where applicable)

---

## Appendix: Test Failure Log Example

```python
"""
Test: test_auth_helper_read_config_corrupted_file
Failure reason: JSONDecodeError not properly converted
Expected: ConfigError exception (custom exception)
Actual: json.JSONDecodeError (raw exception leaking)
Next action: Wrap JSONDecodeError into ConfigError in read_config
Time: Red 3 min, Green 2 min (Total 5 min)
"""
```