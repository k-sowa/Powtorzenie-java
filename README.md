## Odczytywanie danych z pliku CSV za pomocą Apache Commons CSV

### Krok 1: Dodanie zależności

Dodaj następującą zależność do pliku `pom.xml`:

```xml
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.8</version>
</dependency>
```

### Krok 2: Struktura pliku CSV

Upewnij się, że Twój plik CSV ma odpowiednią strukturę. Przykład pliku `events.csv`:

```csv
Name,Time
Event 1,12:00:00
Event 2,14:30:15
Event 3,09:45:00
```

### Krok 3: Klasa modelu danych

Stwórz klasę, która będzie reprezentować dane z pliku CSV. W tym przypadku stworzymy klasę `Event`.

```java
import java.time.LocalTime;

public class Event {
    private String name;
    private LocalTime time;

    public Event(String name, LocalTime time) {
        this.name = name;
        this.time = time;
    }

    public String getName() {
        return name;
    }

    public LocalTime getTime() {
        return time;
    }

    @Override
    public String toString() {
        return "Event{name='" + name + "', time=" + time + '}';
    }
}
```

### Krok 4: Odczytywanie danych z pliku CSV

Stwórz klasę, która będzie odpowiedzialna za odczyt danych z pliku CSV i ich przetwarzanie.

```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;

import java.io.FileReader;
import java.io.IOException;
import java.nio.file.Paths;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public class CsvReader {
    public static List<Event> readEventsFromCsv(String filePath) {
        List<Event> events = new ArrayList<>();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");

        try (FileReader reader = new FileReader(filePath);
             CSVParser csvParser = new CSVParser(reader, CSVFormat.DEFAULT.withHeader())) {

            for (CSVRecord csvRecord : csvParser) {
                String name = csvRecord.get("Name");
                String timeString = csvRecord.get("Time");
                LocalTime time = LocalTime.parse(timeString, formatter);

                events.add(new Event(name, time));
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        return events;
    }

    public static void main(String[] args) {
        String filePath = "events.csv";
        List<Event> events = readEventsFromCsv(filePath);

        for (Event event : events) {
            System.out.println(event);
        }
    }
}
```

### Wyjaśnienie kroków:

1. **Dodanie zależności do Apache Commons CSV**:
   - Dodaj odpowiednią zależność do swojego pliku `pom.xml`, jeśli używasz Maven.

2. **Stworzenie klasy `Event`**:
   - Klasa `Event` przechowuje nazwę wydarzenia oraz czas, w którym się odbywa.

3. **Odczyt danych z pliku CSV**:
   - Użyj klasy `CSVParser` z biblioteki Apache Commons CSV do odczytu i parsowania pliku CSV.
   - Użyj `DateTimeFormatter` do parsowania czasu w formacie `HH:mm:ss`.

4. **Wyświetlanie informacji**:
   - Metoda `main` odczytuje wydarzenia z pliku CSV i wyświetla je na konsoli.


## Odczytywanie godziny w formacie HH:mm:ss z pliku CSV

### Krok 1: Struktura pliku CSV

Twój plik CSV (`events.csv`) powinien mieć kolumny `Name` i `Time`. Przykład zawartości pliku:

```csv
Name,Time
Event 1,12:00:00
Event 2,14:30:15
Event 3,09:45:00
```

### Krok 2: Klasa modelu danych

Upewnij się, że masz klasę `Event`, która przechowuje nazwę wydarzenia i czas.

```java
import java.time.LocalTime;

public class Event {
    private String name;
    private LocalTime time;

    public Event(String name, LocalTime time) {
        this.name = name;
        this.time = time;
    }

    public String getName() {
        return name;
    }

    public LocalTime getTime() {
        return time;
    }

    @Override
    public String toString() {
        return "Event{name='" + name + "', time=" + time + '}';
    }
}
```

### Krok 3: Moduł do odczytywania danych z pliku CSV

Upewnij się, że masz moduł do odczytywania danych z pliku CSV.

```java
import org.apache.commons.csv.CSVFormat;
import org.apache.commons.csv.CSVParser;
import org.apache.commons.csv.CSVRecord;

import java.io.FileReader;
import java.io.IOException;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;

public class CsvReader {
    public static List<Event> readEventsFromCsv(String filePath) {
        List<Event> events = new ArrayList<>();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("HH:mm:ss");

        try (FileReader reader = new FileReader(filePath);
             CSVParser csvParser = new CSVParser(reader, CSVFormat.DEFAULT.withHeader())) {

            for (CSVRecord csvRecord : csvParser) {
                String name = csvRecord.get("Name");
                String timeString = csvRecord.get("Time");
                LocalTime time = LocalTime.parse(timeString, formatter);

                events.add(new Event(name, time));
            }

        } catch (IOException e) {
            e.printStackTrace();
        }

        return events;
    }
}
```

### Krok 4: Użycie modułu do odczytu godzin z pliku CSV

Stwórz klasę, która będzie używać modułu do odczytywania danych z pliku CSV i wyświetlania godzin.

```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        String filePath = "events.csv";
        List<Event> events = CsvReader.readEventsFromCsv(filePath);

        for (Event event : events) {
            System.out.println("Wydarzenie: " + event.getName() + ", Godzina: " + event.getTime());
        }
    }
}
```

### Podsumowanie

1. **Struktura pliku CSV**:
   - Plik CSV powinien mieć kolumny `Name` i `Time` z danymi w formacie `HH:mm:ss`.

2. **Klasa modelu danych (`Event`)**:
   - Klasa do przechowywania nazwy wydarzenia i czasu.

3. **Moduł do odczytywania danych z pliku CSV (`CsvReader`)**:
   - Klasa do odczytywania i parsowania danych z pliku CSV.

4. **Użycie modułu**:
   - Klasa `Main` używa modułu do odczytywania danych i wyświetlania informacji o wydarzeniach i ich godzinach.

Dzięki temu poradnikowi możesz łatwo odczytywać godziny w formacie `HH:mm:ss` z pliku CSV i przetwarzać te dane w swojej aplikacji.
