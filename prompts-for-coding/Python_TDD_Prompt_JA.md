# AIを用いたテスト駆動開発 (TDD) 実行プロンプト - Python版

## 0. 最重要：AI実行時の厳守事項

### 実行フロー（必ず順番通りに実行）

1. **ユーザーから実装タスクを受け取ったら**：
   ```
   [1] TODOリストを作成（TodoWriteツール使用）
   [2] 最初のテストファイルを作成（test_*.pyファイル）
   [3] テストを実行 → 失敗を確認（pytest実行）
   [4] 最小限の実装コードを書く
   [5] テストを実行 → 成功を確認
   [6] リファクタリング実施（改善なしでも明記）
   ```

2. **既存コードの修正時も同様**：
   ```
   [1] 修正内容を検証するテストを先に追加
   [2] テストを実行 → 失敗を確認
   [3] 実装を修正
   [4] テストを実行 → 成功を確認
   ```

3. **テストなしで実装した場合**：
   ```
   [1] 実装コードを即座に削除（gitでロールバック）
   [2] テストから書き直す
   [3] ユーザーにTDD違反を報告
   ```

### 各ステップでの自己宣言（AIが必ず実行）

作業開始時：
```
「TDDで実装を開始します。
1. まず test_[機能名].py を作成します
2. テストを実行して失敗を確認します
3. その後、最小限の実装を行います」
```

違反検出時：
```
「TDD違反に気づきました。実装を一旦コメントアウトします」
→ テストを書く
→ テスト失敗を確認後、コメントアウトを解除
```

### 🔴 絶対に守るべきルール
- **実装コードを書く前に、必ずテストの失敗を確認する**
- **テストファイル（test_*.py）を先に作成する**
- **pytestコマンドで失敗を見てから実装に進む**
- **違反は即座に自己修正し、報告する**

---

## 1. 絶対的禁止事項とTDD違反パターン

### ⚠️ 重要な警告
**実装を開始する前に必ずテストを書くこと。テストなしで実装コードを書いた場合、TDD違反となる。**

### ⛔ 絶対的禁止事項（違反時は即座に作業中止）
1. テストなしでの実装コード記述は**プロンプト違反**として扱う
2. 実装を先に書いた場合、**全作業を破棄して最初からやり直す**
3. TDD違反は**バグと同等の重大な問題**として扱う
4. **プロトタイプ、POC、実験的コードも必ずTDDで実装**
5. 「時間がない」「簡単だから」等の理由での省略は**絶対禁止**

### 🚫 以下の言い訳は一切認めない
- ❌「これはプロトタイプだから...」→ **プロトタイプもTDD必須**
- ❌「簡単な関数だから...」→ **簡単でもTDD必須**
- ❌「時間がないから...」→ **時間がなくてもTDD必須**
- ❌「後でテスト書くから...」→ **後はない、今書く**
- ❌「リファクタリング不要」→ **必ずRefactorフェーズ実行**
- ❌「ヘルパー関数だから...」→ **全ての関数でTDD必須**
- ❌「１行の修正だから...」→ **１行でもTDD必須**

### ⚡ 即座にTDD違反として扱うケース
1. import文やclass定義も含め、実装を1行でも先に書いた
2. テストの実行をスキップして実装に進んだ
3. 「とりあえず動かしてみて...」という思考
4. テストは書いたが、わざと成功するテストを書いた
5. 部分的な実装（たとえ1行でも）を先に書いた

### 部分的な実装への対処
```
もし実装の一部（たとえ1行でも）を先に書いてしまった場合：
1. その瞬間に手を止める
2. 書いた実装を全て削除
3. 「部分的TDD違反を検出しました。実装を削除し、テストから再開します」と宣言
4. テストから書き直す
```

### よくあるTDD違反パターン（避けるべき）

❌ **パターン1**: 「実装してからテストを書く」
❌ **パターン2**: 「複数の機能を一度に実装」
❌ **パターン3**: 「テストの失敗を確認せずに実装」
❌ **パターン4**: 「TODOリスト作成後、すぐに全実装」
❌ **パターン5**: 「テストを書いたが実行せずに実装開始」
❌ **パターン6**: 「簡単だからテストスキップ」
❌ **パターン7**: 「時間超過を理由にTDDスキップ」

### 違反時の対処法
1. **即座に作業を停止**
2. **違反内容を明確に認識・報告**
3. **実装コードを削除またはコメントアウト**
4. **正しいTDDサイクルで再開**

### TDD実行の証跡
各サイクルで以下を必ず記録：
```
[TDD Log]
- Task: implement [機能名]
- Test created: [タイムスタンプ]
- Test failed: [タイムスタンプ] ([失敗理由])
- Implementation: [タイムスタンプ]
- Test passed: [タイムスタンプ]
- Refactored: [タイムスタンプ] ([改善内容または「改善点なし」])
```

---

## 2. TDD サイクル ― 最小単位での反復

### 開始前チェックリスト
- [ ] TODOリストは作成したか？
- [ ] 最初に実装する機能を決定したか？
- [ ] **実装コードは一切書いていないか？**（重要）

### 各ステップ実行前の自己確認（AIが必ず実行）
1. 「私は今からテストを書こうとしているか？」
   - NO → **停止してテストから開始**
2. 「このテストは失敗することを確認したか？」
   - NO → **pytest実行して失敗を確認**
3. 「実装は最小限か？」
   - NO → **削減して最小実装に**

> **★1 テスト = 1 振る舞い原則**\
> *論理的に検証したい振る舞いは 1 つに絞る。複数の `assert` は関連性がある場合のみ許容*。

### ステップ 1: 最も単純な機能から開始

対象: `has_auth_info() -> bool`

#### 第1サイクル: 最初のテストケース

##### ★Red Phase（≤5 分）

```python
def test_auth_helper_has_auth_info_no_file(mock_filesystem):
    """
    失敗理由: AuthHelper が未実装 → ImportError
    期待: False, 実際: undefined (ImportError)
    次のアクション: 最小実装（return False）
    """
    mock_filesystem.exists.return_value = False
    
    # helper = AuthHelper(mock_filesystem, mock_clock)
    # assert helper.has_auth_info() is False
```

##### ★Green Phase（≤5 分）

```python
class AuthHelperImpl:
    def __init__(self, fs: FileSystem, clock: Clock):
        self._fs = fs
        self._clock = clock
    
    def has_auth_info(self) -> bool:
        return False  # ハードコードでもテストは通る！
```

##### ★Refactor Phase（≤3 分）
- **必ず実行**（改善点がない場合は「改善点なし」と明記）
- この時点では特にリファクタリングなし → 「改善点なし」

#### 第2サイクル: 2つ目のテストケースによる一般化（Triangulation）

##### ★Red Phase（≤5 分）

```python
def test_auth_helper_has_auth_info_file_exists(mock_filesystem, mock_clock):
    """
    失敗理由: 常にFalseを返す実装のため
    期待: True, 実際: False
    次のアクション: ファイル存在チェックの実装
    所要時間: Red 2分
    """
    mock_filesystem.exists.return_value = True
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    assert helper.has_auth_info() is True  # この時点でRed！
```

##### ★Green Phase（≤5 分）

```python
def has_auth_info(self) -> bool:
    # ハードコードを削除し、一般化実装へ
    return self._fs.exists(Path.home() / ".config" / "auth.json")
```

##### ★Refactor Phase（≤3 分）
- **必ず実行**

```python
class AuthHelperImpl:
    CONFIG_PATH = Path.home() / ".config" / "auth.json"  # マジックパスを定数化
    
    def __init__(self, fs: FileSystem, clock: Clock):
        self._fs = fs
        self._clock = clock
    
    def has_auth_info(self) -> bool:
        return self._fs.exists(self.CONFIG_PATH)
```

> **ポイント**: 1つのメソッドに対して複数のテストケースを書くことで、段階的に正しい実装に導きます。

### ステップ 2: 次のメソッド実装（read_config）とエラーハンドリング

対象: `read_config() -> Config`

> **注**: 各メソッドの実装は独立したRed-Green-Refactorサイクルで行います。
> エラーハンドリングは、そのメソッドの基本実装完了後、Phase 2（基本エラー）で追加します。

#### Pythonのエラーハンドリングパターン

```python
import pytest
from pathlib import Path

# Phase 1: Happy Path
def test_auth_helper_read_config_success(mock_filesystem, mock_clock):
    """
    失敗理由: read_config メソッドが未実装
    期待: Config オブジェクト, 実際: NotImplementedError
    次のアクション: 最小実装
    """
    mock_filesystem.exists.return_value = True
    mock_filesystem.read.return_value = b'{"api_key": "test", "endpoint": "http://test"}'
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    config = helper.read_config()
    
    assert config.api_key == "test"
    assert config.endpoint == "http://test"

# Phase 2: 基本エラー（Happy Path実装後に追加）
def test_auth_helper_read_config_file_not_found(mock_filesystem, mock_clock):
    """
    失敗理由: FileNotFoundError が発生していない
    期待: FileNotFoundError, 実際: None
    次のアクション: ファイル不在時の例外処理実装
    """
    mock_filesystem.exists.return_value = False
    mock_filesystem.read.side_effect = FileNotFoundError("config not found")
    
    helper = AuthHelperImpl(mock_filesystem, mock_clock)
    
    with pytest.raises(FileNotFoundError) as exc_info:
        helper.read_config()
    
    assert "config not found" in str(exc_info.value)
```

### ステップ 3 以降

- メソッド単位に同サイクルを繰り返す
- 段階的拡張は §5 を参照

---

## 3. 要件と環境設定

### 要件

- ここに開発対象機能・非機能要件を記述
- Python 3.9+／`pytest` 環境を前提
- 型ヒント（`typing`）の積極的使用

### 実行方針

**重要:** 以下を厳守し、各ステップ完了後のみ次工程へ進むこと。

### 3‑1. 初期設計フェーズ（10 分以内）

- 必要最小限の **抽象基底クラス（ABC）** または **Protocol** を定義し、実装詳細は後回し
- **外部依存**（FileSystem, Clock, DB, API…）も必ず抽象化し、DIP を徹底
- **時間超過時**: タスクを分割してTDD継続（決してTDDをスキップしない）

#### ProtocolとABCの使い分け基準

| 使用場面 | Protocol | ABC |
|---------|----------|-----|
| 型チェックのみ必要 | ✓ 推奨 | |
| 実装の継承が必要 | | ✓ 推奨 |
| ダックタイピング | ✓ 推奨 | |
| 複数継承が必要 | ✓ 推奨 | |
| 実行時の型検証 | runtime_checkable | isinstance可 |

```python
from abc import ABC, abstractmethod
from typing import Protocol, runtime_checkable
from datetime import datetime
from pathlib import Path

# Protocol使用例（依存の注入先として推奨）
@runtime_checkable
class FileSystem(Protocol):
    def exists(self, path: Path) -> bool: ...
    def read(self, path: Path) -> bytes: ...

class Clock(Protocol):
    def now(self) -> datetime: ...

# ABC使用例（実装の階層構造がある場合）
class AuthHelper(ABC):
    @abstractmethod
    def has_auth_info(self) -> bool:
        pass
    
    @abstractmethod
    def read_config(self) -> dict:
        pass
```

### 3‑2. モック戦略

```python
# pytest-mockの活用（推奨）
from unittest.mock import Mock, MagicMock
import pytest

# 手動モック実装例
class MockFileSystem:
    def __init__(self):
        self.exists = Mock(return_value=False)
        self.read = Mock(return_value=b'{}')

# fixtureパターン（推奨）
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

## 4. 段階的な機能拡張

| Phase | 目的         | 範囲                       |
| ----- | ---------- | ------------------------ |
| 1     | Happy Path | 正常系のみ                    |
| 2     | 基本エラー      | I/O・権限・型エラー・例外ハンドリング    |
| 3     | エッジケース     | None値・空データ・境界値・並行処理     |
| 4     | パフォーマンス    | プロファイリング・最適化（必要時のみ）    |

### 4‑1. 並行処理のテスト戦略

```python
import asyncio
import threading
from concurrent.futures import ThreadPoolExecutor
import pytest

def test_auth_helper_thread_safe(mock_filesystem, mock_clock):
    """スレッドセーフティのテスト"""
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
    """非同期並行アクセステスト"""
    helper = AsyncAuthHelper(mock_filesystem, mock_clock)
    
    tasks = [helper.has_auth_info() for _ in range(100)]
    results = await asyncio.gather(*tasks)
    
    assert all(r is False for r in results)
```

### 4‑2. プロパティベーステスト（Hypothesis）

```python
from hypothesis import given, strategies as st

class TestAuthHelper:
    @given(
        path=st.text(min_size=1, max_size=255),
        exists=st.booleans()
    )
    def test_has_auth_info_property(self, path, exists, mock_clock):
        """プロパティベーステスト: 任意の入力で正常動作"""
        mock_fs = Mock(spec=FileSystem)
        mock_fs.exists.return_value = exists
        
        helper = AuthHelperImpl(mock_fs, mock_clock)
        result = helper.has_auth_info()
        
        assert isinstance(result, bool)
```

---

## 5. 制約事項と品質基準

### ❌ 禁止

- 複数テスト同時作成／テストなし実装／先取り実装
- 30 行超を一度に実装
- 裸の `except:` 節（必ず特定の例外をキャッチ）
- 型ヒントなしの public メソッド
- `# type: ignore` の濫用
- **ヘルパー関数・ユーティリティもテストなしでの実装禁止**

### ✅ 必須

- 各サイクル所要時間を記録
- テスト失敗理由の具体的な明記
- 5 分超実装は分割
- すべての **public メソッド** にテスト
- **カバレッジ80% 以上**（段階的達成）
- 例外パスのテストカバレッジ
- 型ヒントの完全性（`mypy --strict`）
- **Refactorフェーズは必ず実行**（スキップ不可）

### ★Refactor チェックリスト（毎サイクル自問）

1. **SRP** 違反はないか
2. 循環インポートはないか
3. 外部依存は **Protocol/ABC 経由**か
4. 公開 API は最小か（`_` プレフィックス活用）
5. 名前が意図を表すか（PEP 8準拠）
6. 例外は適切に伝搬されているか
7. 型安全性は保たれているか
8. イミュータブルなデータ構造を活用しているか

### ★静的解析ツール（CI Exit 条件）

```bash
# 型チェック
mypy --strict src/

# Linting
ruff check src/
# または
flake8 src/ --max-complexity=10

# セキュリティ
bandit -r src/

# カバレッジ
pytest --cov=src --cov-report=term-missing --cov-fail-under=80

# 循環複雑度
radon cc src/ -nc

# コードフォーマット（自動修正）
black src/
isort src/
```

---

## 6. 設計パターンとPython固有事項

### ★依存注入の実装例

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
        self._lock = RLock()  # スレッドセーフティ
        self._config_cache: Optional[Config] = None
    
    def has_auth_info(self) -> bool:
        with self._lock:
            return self._fs.exists(Path.home() / ".config" / "auth.json")
    
    def read_config(self) -> Config:
        with self._lock:
            if self._config_cache is None:
                # 実装
                pass
            return self._config_cache
```

### 型エイリアスとGenericsの活用

```python
from typing import TypeVar, Generic, TypeAlias, NewType

# 型エイリアスで意図を明確化
ConfigDict: TypeAlias = dict[str, str | int | bool]
UserId = NewType('UserId', str)

# Genericsで再利用可能な抽象化
T = TypeVar('T')

class Repository(Protocol, Generic[T]):
    def find(self, id: str) -> T | None: ...
    def save(self, entity: T) -> None: ...

class ConfigRepository:
    """Repository[Config]の具象実装"""
    def find(self, id: str) -> Config | None:
        # 実装
        pass
    
    def save(self, entity: Config) -> None:
        # 実装
        pass

# テストでの活用例
def test_repository_generic_behavior():
    """Genericsを使った汎用的なテスト"""
    repo: Repository[Config] = ConfigRepository()
    assert repo.find("test") is None
```

### デコレータパターンの活用

```python
from functools import wraps
import logging

def with_retry(max_attempts: int = 3):
    """リトライ機能のデコレータ"""
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

### コンテキストマネージャの活用

```python
from contextlib import contextmanager
from typing import Generator

@contextmanager
def temporary_config(helper: AuthHelper, config: Config) -> Generator[None, None, None]:
    """一時的な設定変更のコンテキストマネージャ"""
    original = helper._config_cache
    try:
        helper._config_cache = config
        yield
    finally:
        helper._config_cache = original
```

---

## 7. コミット戦略とAI連携

### コミット戦略

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

### AI 連携フロー ★Python特化

TDDの各フェーズでAIを効果的に活用するための指針です。

### 7‑1. サンプルプロンプト集（TDDフェーズ別）

#### Red Phase（テスト作成時）
- **基本テスト**: 「このProtocolのpytestパラメータ化テスト骨子を生成」
- **例外ケース**: 「考慮すべき例外とpytest.raisesでの検証方法を提案」
- **プロパティテスト**: 「Hypothesisを使ったプロパティベーステストを提案」

#### Green Phase（実装時）
- **最小実装**: 「このテストをパスする最小実装（ハードコード可）を提案」
- **型安全**: 「mypy --strictでエラーが出ない型ヒントを提案」
- **非同期対応**: 「async/await対応とpytest-asyncioでのテスト例」

#### Refactor Phase（改善時）
- **構造改善**: 「SOLID原則に基づくリファクタリング案」
- **性能改善**: 「プロファイリング結果に基づく最適化案」
- **Pythonic化**: 「より慣用的なPythonコードへの改善案」

### 7‑2. AI‑Output Gate（自動＋人レビュー）

AIが生成したコードは、以下のゲートを通過するまでコミット禁止：

1. `mypy --strict` → 2. `pytest` → 3. `ruff check` → 4. `bandit` → 5. `pytest --cov` → 6. **Human review**

#### 品質チェックの実行例

```bash
# AIが生成したコードを検証
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

#### Human Reviewチェックリスト

- [ ] AIの生成コードは要求仕様を満たしているか
- [ ] 不要な複雑性が含まれていないか（YAGNI原則）
- [ ] エラーハンドリングは適切か
- [ ] 型ヒントは正確で完全か
- [ ] テストケースは十分な網羅性があるか

> AI生成コードは必ずGateを通すことで、型エラー・セキュリティ脆弱性・Hallucinationを防止します。

---

## 8. 完了基準

- すべての機能が **Red→Green→Refactor** サイクルで実装済み
- Refactor チェックリスト違反ゼロ
- CI で mypy / ruff / bandit / pytest-cov 80% クリア
- 例外パスのテストカバレッジ 100%
- 並行処理を含む場合はスレッドセーフティテスト成功
- 型ヒント完全性（`mypy --strict` エラーゼロ）
- AI‑Output Gate ログ完備（プロンプト・レスポンス履歴）
- 必要に応じてプロファイリング結果が要件を満たす

---

## 付録: テスト失敗ログの記載例

```python
"""
テスト: test_auth_helper_read_config_corrupted_file
失敗理由: JSONDecodeError が適切に変換されていない
期待: ConfigError 型の例外（カスタム例外）
実際: json.JSONDecodeError（生の例外が漏れている）
次のアクション: read_config 内で JSONDecodeError を ConfigError にラップ
所要時間: Red 3分, Green 2分（計5分）
"""
```