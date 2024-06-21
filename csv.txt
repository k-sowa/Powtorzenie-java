Poradnik: Odczytywanie danych z pliku CSV za pomocą Apache Commons CSV
Krok 1: Dodanie zależności
Jeśli używasz Maven, dodaj następującą zależność do pliku pom.xml:

```
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-csv</artifactId>
    <version>1.8</version>
</dependency>
```
Krok 2: Struktura pliku CSV
Upewnij się, że Twój plik CSV ma odpowiednią strukturę. Przykład pliku events.csv:

```
Name,Time
Event 1,12:00:00
Event 2,14:30:15
Event 3,09:45:00
```
Krok 3: Klasa modelu danych
Stwórz klasę, która będzie reprezentować dane z pliku CSV. W tym przypadku stworzymy klasę Event.

```
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
Krok 4: Odczytywanie danych z pliku CSV
Stwórz klasę, która będzie odpowiedzialna za odczyt danych z pliku CSV i ich przetwarzanie.

```
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
Wyjaśnienie kroków:
Dodanie zależności do Apache Commons CSV:

Dodaj odpowiednią zależność do swojego pliku pom.xml, jeśli używasz Maven.
Stworzenie klasy Event:

Klasa Event przechowuje nazwę wydarzenia oraz czas, w którym się odbywa.
Odczyt danych z pliku CSV:

Użyj klasy CSVParser z biblioteki Apache Commons CSV do odczytu i parsowania pliku CSV.
Użyj DateTimeFormatter do parsowania czasu w formacie HH:mm:ss.
Wyświetlanie informacji:

Metoda main odczytuje wydarzenia z pliku CSV i wyświetla je na konsoli.
Podsumowanie
