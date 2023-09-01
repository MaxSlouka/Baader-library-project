# Baader-library-project

# Dokumentace knihovny ArgumentParser
## Úvod
Tato knihovna poskytuje jednoduchý způsob zpracování vstupních argumentů a volitelných voleb v konzolových aplikacích. Je navržena tak, aby byla flexibilní a umožňovala programátorům definovat různé typy voleb včetně vlastních datových typů.

## Použití
Registrace voleb:

Pro začátek použití knihovny je třeba zaregistrovat všechny významné volby, které aplikace podporuje. To se provádí pomocí metody addOption. Zde je příklad registrace volby pro vlastní datový typ:

CustomType customDefault = new MyCustomObject();
Option<CustomType> customOption = new CustomOption("custom", false, "Custom option", new String[]{"c"}, customDefault, value -> true);
parser.addOption(customOption);

Zpracování argumentů:

Jakmile jsou všechny volby zaregistrovány, můžete přejít k zpracování vstupních argumentů voláním metody parse s polem vstupních argumentů:

parser.parse(args);


Získání hodnot voleb:

K získání hodnot voleb použijte metodu getOptionValue s názvem volby:

CustomType customValue = parser.getOptionValue("custom");


Manipulace s vlastním datovým typem:

Po získání hodnoty vlastní volby můžete provádět operace s vlastním datovým typem, například volat metody a provádět další operace.


## Třídy
Option<T>

name: Název volby.

required: Určuje, zda je volba povinná.

description: Popis volby.

aliases: Pole aliasů pro volbu.

defaultValue: Výchozí hodnota volby.
validator: Validátor pro ověření hodnoty volby.<br>
OptionValidator<T>
Rozhraní pro validaci hodnot volby.
CustomType
Rozhraní pro definici vlastního datového typu.


// Enum pro definici typu volby
enum OptionType {
    INTEGER, STRING, BOOLEAN, ENUM, CUSTOM
}

// Rozhraní pro vlastní datový typ
interface CustomType {
    // Metody pro manipulaci s vlastním typem
    void customMethod();
}

// Abstraktní třída pro volbu
abstract class Option<T> {
    private String name;
    private boolean required;
    private String description;
    private String[] aliases;
    private T defaultValue;
    private OptionValidator<T> validator;

    public Option(String name, boolean required, String description, String[] aliases, T defaultValue, OptionValidator<T> validator) {
        this.name = name;
        this.required = required;
        this.description = description;
        this.aliases = aliases;
        this.defaultValue = defaultValue;
        this.validator = validator;
    }

    public String getName() {
        return name;
    }

    public boolean isRequired() {
        return required;
    }

    public String getDescription() {
        return description;
    }

    public String[] getAliases() {
        return aliases;
    }

    public T getDefaultValue() {
        return defaultValue;
    }

    public OptionValidator<T> getValidator() {
        return validator;
    }

    public abstract T parseValue(String value);

    // Další metody a atributy pro všechny typy voleb
}

// Rozhraní pro validaci hodnot volby
interface OptionValidator<T> {
    boolean validate(T value);
}

// Implementace vlastního datového typu
class MyCustomObject implements CustomType {
    // Implementace vlastního datového typu
    @Override
    public void customMethod() {
        // Implementace vlastní metody
    }
}

// Třída reprezentující volbu pro vlastní datový typ
class CustomOption extends Option<CustomType> {
    public CustomOption(String name, boolean required, String description, String[] aliases, CustomType defaultValue, OptionValidator<CustomType> validator) {
        super(name, required, description, aliases, defaultValue, validator);
    }

    @Override
    public CustomType parseValue(String value) {
        // Implementace parsování hodnoty pro vlastní datový typ
        // Zde byste měli zpracovat řetězec 'value' a vytvořit instanci MyCustomObject nebo jiného vlastního typu
        return new MyCustomObject();
    }
}

// Třída pro zpracování vstupních argumentů
class ArgumentParser {
    private List<Option<?>> options;

    public ArgumentParser() {
        options = new ArrayList<>();
    }

    public <T> void addOption(Option<T> option) {
        options.add(option);
    }

    public void parse(String[] args) {
        // Implementace zpracování argumentů podle zaregistrovaných voleb
    }

    public <T> T getOptionValue(String name) {
        // Implementace získání hodnoty volby podle jména
        return null;
    }
}

// Ukázková aplikace s použitím knihovny pro zpracování argumentů
public class MyApp {
    public static void main(String[] args) {
        ArgumentParser parser = new ArgumentParser();

        // Registrace voleb
        CustomType customDefault = new MyCustomObject();
        Option<CustomType> customOption = new CustomOption("custom", false, "Custom option", new String[]{"c"}, customDefault, value -> true);
        parser.addOption(customOption);

        // Zpracování argumentů
        parser.parse(args);

        // Získání hodnoty vlastní volby
        CustomType customValue = parser.getOptionValue("custom");
        customValue.customMethod();
    }
}


