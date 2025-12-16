# Python Conventions

## Stack Overview
- **Python Version**: 3.11+
- **Type Hints**: Required (use Pydantic for validation)
- **Async**: asyncio for I/O-bound operations
- **Package Management**: Poetry or pip with requirements.txt
- **Linting**: Ruff, Black for formatting

## Project Structure
```
src/
├── models/           # Pydantic models
├── services/         # Business logic
├── api/              # API endpoints (FastAPI/Flask)
├── utils/            # Utility functions
├── config/           # Configuration management
└── tests/            # Test files
```

## Pydantic Models
```python
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

class Order(BaseModel):
    id: int
    app_id: int = Field(alias="appId")
    status: str
    created_at: datetime

    class Config:
        populate_by_name = True  # Allow both snake_case and camelCase
```

## Async Patterns
```python
import asyncio
from typing import List

async def fetch_orders(app_id: int) -> List[Order]:
    async with aiohttp.ClientSession() as session:
        async with session.get(f"/api/orders?app_id={app_id}") as response:
            data = await response.json()
            return [Order(**item) for item in data]

# Running multiple async operations
async def fetch_all():
    results = await asyncio.gather(
        fetch_orders(1),
        fetch_orders(2),
        fetch_orders(3),
    )
    return results
```

## Configuration
```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    api_key: str
    database_url: str
    debug: bool = False

    class Config:
        env_file = ".env"

settings = Settings()
```

## Error Handling
```python
from typing import TypeVar, Generic
from pydantic import BaseModel

T = TypeVar("T")

class Result(BaseModel, Generic[T]):
    success: bool
    data: Optional[T] = None
    error: Optional[str] = None

def safe_operation() -> Result[Order]:
    try:
        order = process_order()
        return Result(success=True, data=order)
    except Exception as e:
        return Result(success=False, error=str(e))
```

## Logging
```python
import logging

logger = logging.getLogger(__name__)

def process_order(order_id: int):
    logger.info(f"Processing order {order_id}")
    try:
        # Process
        logger.debug(f"Order {order_id} processed successfully")
    except Exception as e:
        logger.error(f"Failed to process order {order_id}: {e}")
        raise
```

## Testing
```python
import pytest
from unittest.mock import AsyncMock, patch

@pytest.mark.asyncio
async def test_fetch_orders():
    mock_response = [{"id": 1, "appId": 1, "status": "active"}]

    with patch("aiohttp.ClientSession.get") as mock_get:
        mock_get.return_value.__aenter__.return_value.json = AsyncMock(
            return_value=mock_response
        )

        orders = await fetch_orders(1)
        assert len(orders) == 1
        assert orders[0].id == 1
```

## MCP Server Patterns
For Model Context Protocol servers:

```python
from mcp import Server, Tool

server = Server("violet-commerce")

@server.tool()
async def search_products(query: str, limit: int = 10) -> list:
    """Search for products in the catalog."""
    # Implementation
    pass

@server.tool()
async def get_order(order_id: int) -> dict:
    """Get order details by ID."""
    # Implementation
    pass
```

## Commands
```bash
# Development
python -m pytest              # Run tests
python -m pytest --cov        # With coverage
ruff check .                  # Lint
black .                       # Format

# Type checking
mypy src/
```
