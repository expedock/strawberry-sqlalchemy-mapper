# strawberry-sqlalchemy-mapper


Strawberry-sqlalchemy-mapper is the simplest way to implement autogenerated strawberry types for columns and relationships in SQLAlchemy models.


- Instead of manually listing every column and relationship in a SQLAlchemy model, strawberry-sqlalchemy-mapper
lets you decorate a class declaration and it will automatically generate the necessary strawberry fields
for all columns and relationships in the given model.

- Native support for most of SQLAlchemy's most common types.

- Extensible to arbitrary custom SQLAlchemy types.

- Support for SQLAlchemy >=1.4.x

- Lightweight and fast.

## Getting Started

First, define your sqlalchemy model:

```python
# models.py
from sqlalchemy import Column, Integer, String
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

class Employee(Base):
    __tablename__ = 'employee'
    id = Column(UUID, primary_key=True)
    name = Column(String, nullable=False)
```

Next, decorate a type with `strawberry_sqlalchemy_mapper.type()`
to register it as a strawberry type for the given SQLAlchemy model.
This will automatically add fields for the model's columns, relationships, association proxies,
and hybrid properties. For example:

```python
# elsewhere
strawberry_sqlalchemy_mapper = StrawberrySQLAlchemyMapper()
@strawberry_sqlalchemy_mapper.type(models.Employee)
class Employee:
    pass
```

Several examples to help you get started are provided in the `examples` directory.

## Limitations

Natively supports the following SQLAlchemy types:

```python
Integer: int,
Float: float,
BigInteger: int,
Numeric: Decimal,
DateTime: datetime,
Date: date,
Time: time,
String: str,
Text: str,
Boolean: bool,
Unicode: str,
UnicodeText: str,
SmallInteger: int,
SQLAlchemyUUID: uuid.UUID,
VARCHAR: str,
ARRAY[T]: List[T] # PostgreSQL array
Enum: (the Python enum it is mapped to, which should be @strawberry.enum-decorated)
```

Additional types can be supported by passing `extra_sqlalchemy_type_to_strawberry_type_map`,
although support for `TypeDecorator` types is untested.

Association proxies are expected to be of the form `association_proxy('relationship1', 'relationship2')`,
i.e., both properties are expected to be relationships.

## Contributing

We encourage you to contribute to strawberry-sqlalchemy-mapper! Any contributions you make are greatly appreciated.

If you have a suggestion that would make this better, please fork the repo and create a pull request. Don't forget to give the project a star! Thanks again!

1. Fork the Project
2. Create your Feature Branch (git checkout -b feature)
3. Commit your Changes (git commit -m 'Add some feature')
4. Push to the Branch (git push origin feature)
5. Open a Pull Request


### Prerequisites

This project uses `pre-commit`_, please make sure to install it before making any
changes::

    pip install pre-commit
    cd strawberry-sqlalchemy-mapper
    pre-commit install

It is a good idea to update the hooks to the latest version::

    pre-commit autoupdate

Don't forget to tell your contributors to also install and use pre-commit.

### Installation

```bash
pip install -r requirements.txt
```

### Test

```bash
pytest
```

## ⚖️ LICENSE

MIT © [strawberry-sqlalchemy-mapper](LICENSE.txt)