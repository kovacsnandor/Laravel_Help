# Sorbarendezés
## Sort bubble

```cs
public int[] SorterItems(int[] numbers)
{
    int n = numbers.Length;
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = 0; j < n - i - 1; j++)
        {
            if (numbers[j] > numbers[j + 1])
            {
                // Elemek cseréje
                int temp = numbers[j];
                numbers[j] = numbers[j + 1];
                numbers[j + 1] = temp;
            }
        }
    }
    return numbers;
}
```

## Sort coctail

```cs
public int[] SorterItems(int[] numbers)
{
    int n = numbers.Length;
    bool swapped = true;
    int start = 0, end = n - 1;

    while (swapped == true)
    {
        swapped = false;

        // Balról jobbra haladva
        for (int i = start; i < end; i++)
        {
            if (numbers[i] > numbers[i + 1])
            {
                // Cseréljük meg az elemeket
                int temp = numbers[i];
                numbers[i] = numbers[i + 1];
                numbers[i + 1] = temp;
                swapped = true;
            }
        }

        // Ha nem történt csere, akkor a tömb már rendezett
        if (swapped == false)
            break;

        // Jobbról balra haladva
        swapped = false;
        end--;

        for (int i = end - 1; i >= start; i--)
        {
            if (numbers[i] > numbers[i + 1])
            {
                // Cseréljük meg az elemeket
                int temp = numbers[i];
                numbers[i] = numbers[i + 1];
                numbers[i + 1] = temp;
                swapped = true;
            }
        }
        start++;
    }

    return numbers;
}
```

## Sort quick

```cs
public int[] SorterItems(int[] numbers)
{
    int length = numbers.Length;
    Stack<int[]> stack = new Stack<int[]>();
    stack.Push(new int[] { 0, length - 1 });

    while (stack.Count > 0)
    {
        int[] range = stack.Pop();
        int low = range[0], high = range[1];

        int pi = Partition(numbers, low, high);

        // A bal és jobb részintervallumokat a stack-re tesszük
        if (pi > low)
        {
            stack.Push(new int[] { low, pi - 1 });
        }
        if (pi < high)
        {
            stack.Push(new int[] { pi + 1, high });
        }
    }

    return numbers; ;
}

private int Partition(int[] numbers, int low, int high)
{
    int pivot = numbers[high];
    int i = (low - 1);

    for (int j = low; j < high; j++)
    {
        // Ha az aktuális elem kisebb vagy egyenlő a pivotnál
        if (numbers[j] <= pivot)
        {
            i++;

            // Cseréljük meg az elemeket
            int temp = numbers[i];
            numbers[i] = numbers[j];
            numbers[j] = temp;
        }
    }

    // Helyezzük a pivot elemet a megfelelő helyre
    int temp1 = numbers[i + 1];
    numbers[i + 1] = numbers[high];
    numbers[high] = temp1;

    return i + 1;
}
```