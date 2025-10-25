# Библиотечная система на Python с Pydantic

Простая система управления библиотекой, реализованная с использованием Pydantic для валидации данных и типизации Python.

## Функциональность

- **Управление книгами**: добавление, поиск, выдача и возврат книг
- **Управление пользователями**: регистрация пользователей с валидацией email
- **Валидация данных**: автоматическая проверка корректности вводимых данных
- **Категории книг**: поддержка категоризации книг с обязательной валидацией
- **Отслеживание доступности**: контроль статуса книг (доступна/выдана)

## Модели данных

### Book
- `title` (str): Название книги
- `author` (str): Автор книги
- `year` (int): Год издания
- `available` (bool): Статус доступности (по умолчанию True)
- `categories` (List[str]): Список категорий книги

**Валидация**: Книга должна иметь как минимум одну категорию

### User
- `name` (str): Имя пользователя
- `email` (EmailStr): Email с автоматической валидацией
- `membership_id` (str): ID членства

### Library
- `books` (List[Book]): Список книг в библиотеке
- `users` (List[User]): Список зарегистрированных пользователей

## API функций

### `add_book(books: List[Book], book: Book) -> List[Book]`
Добавляет новую книгу в список библиотеки.

### `find_book(books: List[Book], title: str) -> Optional[Book]`
Ищет книгу по названию (регистронезависимо).

### `is_book_borrow(book: Book) -> bool`
Выдает книгу, если она доступна. Выбрасывает исключение `BookNotAvailable` если книга недоступна.

### `return_book(book: Book) -> bool`
Возвращает книгу в библиотеку, устанавливая статус доступности в True.

### `library.total_books() -> int`
Возвращает общее количество книг в библиотеке.

## Исключения

### `BookNotAvailable`
Вызывается при попытке выдать книгу, которая уже выдана.

## Пример использования

```python
# Создание книг и пользователей
book1 = Book(
    title="1984", 
    author="Джордж Оруэлл", 
    year=1949, 
    available=True, 
    categories=["Роман", "Антиутопия"]
)
book2 = Book(
    title="Война и мир", 
    author="Лев Толстой", 
    year=1869, 
    available=True, 
    categories=["Классика"]
)

user1 = User(
    name="Даниил", 
    email="danil@example.com", 
    membership_id="U001"
)

# Создание библиотеки
library = Library(books=[book1, book2], users=[user1])

# Использование функций
print("Всего книг в библиотеке:", library.total_books())

found = find_book(library.books, "1984")
print("Найдена книга:", found.title if found else "не найдена")

# Выдача книги
try:
    is_book_borrow(found)
    print(f"Книга '{found.title}' выдана.")
except BookNotAvailable as e:
    print("Ошибка:", e)
