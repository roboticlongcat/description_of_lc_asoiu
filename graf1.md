# Операторно-параметрическая схема модели ремонта "Прибор-Мастер"

## Схема "Прибор"

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {
  'primaryColor': '#ffcc00',
  'primaryTextColor': '#000',
  'primaryBorderColor': '#7C0000',
  'lineColor': '#F8B229',
  'secondaryColor': '#006100',
  'tertiaryColor': '#fff'
}}}%%
flowchart LR
    title[<em>Блок ПРИБОР</em>]
    
    h1(["ВРЕМЯ=ТСЛОМ"]) ==> h2["сост:=<br>сломан"]
    h2 ==> h3(["режим:=<br>работа"])
    h3 ==> h5["мастер:=занят<br>Трем:=func(x)"]
    h5 ==> h6(["ВРЕМЯ=Трем"])
    h6 ==> h7["сост:=рабочий<br>мастер:=своб<br>Тслом:=func(x)"]
    h7 ==> h1
    
    %% Параметры
    h2 -.- par3((сост))
    h7 -.- par3
    par1((режим)) -.- h3
    par2((мастер)) -.- h5
    h7 -.- par2
    h5 -.- par4((Трем))
    par4 -.- h6
    Ini@{shape: braces, label: "I::"} -.- h1
    HTf@{shape: braces, label: "I::Tclom = 100"}
    
    %% Стили связей
    h2 e11@==> h3
    h5 e20@==> h6
    h7 e10@==> h1
    e10@{ curve: linear}
    e11@{ curve: natural}
    e20@{ curve: stepAfter}
    
    %% Стили узлов
    classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
    classDef state fill:#9e8,stroke:#333,stroke-width:1px;
    class h5,h2,h7 state;
    class h1,h3,h6 cond;
    style title fill:yellow,stroke:red;
    style par1 fill:#fcc,stroke:#111,stroke-width:2px;
    style par2 fill:#fae,stroke:#bbb,stroke-width:2px;
    style par4 fill:#ccc,stroke:#555,stroke-width:2px;
    
    %% Ссылки
    click par2 href "https://iu5.bmstu.ru" "переход для Мастера" _blank;
    click par4 href "https://mermaid.js.org" "информация о параметре Трем" _blank;
```

## Схема "Мастер"

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {
  'primaryColor': '#ffcc00',
  'primaryTextColor': '#000',
  'primaryBorderColor': '#7C0000',
  'lineColor': '#F8B229',
  'secondaryColor': '#006100',
  'tertiaryColor': '#fff'
}}}%%
flowchart LR
    title_m[<em>Блок МАСТЕР</em>]
    
    h10(["ВРЕМЯ=Тцикл"]) ==> h11["мастер:=своб"]
    h11 ==> h12{"заявка<br>есть?"}
    h12 ==>|"да"| h13["мастер:=занят"]
    h13 ==> h14(["ВРЕМЯ=Траб"])
    h14 ==> h15{"мастер<br>=>..."}
    h15 ==>|"...= занят"| h16["Трем:=Трем:+<br>Траб-ВРЕМЯ"]
    h15 ==>|"...= свобод"| h11
    h16 ==> h10
    
    h12 ==>|"нет"| h10
    
    %% Параметры
    h11 -.- par_m1((сост_мастера))
    h13 -.- par_m1
    h15 -.- par_m2((мастер))
    
    %% Стили
    classDef navig fill:#eda,stroke:#333,stroke-width:1px;
    classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
    classDef state fill:#9e8,stroke:#333,stroke-width:1px;
    class h11,h13,h16 state;
    class h10,h14 cond;
    class h12,h15 navig;
    style title_m fill:yellow,stroke:red;
```

## Объединенная схема

```mermaid
%%{init: {'theme': 'base', 'themeVariables': {
  'primaryColor': '#ffcc00',
  'primaryTextColor': '#000',
  'primaryBorderColor': '#7C0000',
  'lineColor': '#F8B229',
  'secondaryColor': '#006100',
  'tertiaryColor': '#fff'
}}}%%
flowchart TD
    title_all[<em>Модель ремонта ПРИБОР-МАСТЕР</em>]
    
    subgraph equip [Подграф: Прибор]
        direction TB
        e1(["ВРЕМЯ=ТСЛОМ"]) ==> e2["сост:=сломан"]
        e2 ==> e3(["режим:=работа"])
        e3 ==> e4["мастер:=занят<br>Трем:=func(x)"]
        e4 ==> e5(["ВРЕМЯ=Трем"])
        e5 ==> e6["сост:=рабочий<br>мастер:=своб<br>Тслом:=func(x)"]
        e6 ==> e1
        
        e2 -.- p1((сост))
        e6 -.- p1
        p2((режим)) -.- e3
    end
    
    subgraph master [Подграф: Мастер]
        direction TB
        m1(["ВРЕМЯ=Тцикл"]) ==> m2["мастер:=своб"]
        m2 ==> m3{"заявка<br>есть?"}
        m3 ==>|"да"| m4["мастер:=занят"]
        m4 ==> m5(["ВРЕМЯ=Траб"])
        m5 ==> m6{"мастер<br>=>..."}
        m6 ==>|"...= занят"| m7["Трем:=Трем:+<br>Траб-ВРЕМЯ"]
        m6 ==>|"...= свобод"| m2
        m7 ==> m1
        m3 ==>|"нет"| m1
        
        m2 -.- p3((сост_мастера))
        m4 -.- p3
        m6 -.- p4((мастер))
    end
    
    %% Связи между подграфами
    equip --> master
    master --> equip
    
    %% Общие параметры
    p5((параметры<br>ремонта)) -.- e4
    p5 -.- m7
    
    %% Начальные значения
    Ini@{shape: braces, label: "I::"} -.- e1
    HTf@{shape: braces, label: "I::Tclom = 100"} -.- e1
    Init_m@{shape: braces, label: "I::Тцикл = 50"} -.- m1
    
    %% Стили
    classDef cond fill:#bee,stroke:#aaa,stroke-width:1px;
    classDef state fill:#9e8,stroke:#333,stroke-width:1px;
    classDef navig fill:#eda,stroke:#333,stroke-width:1px;
    class e2,e4,e6,m2,m4,m7 state;
    class e1,e3,e5,m1,m5 cond;
    class m3,m6 navig;
    
    style title_all fill:yellow,stroke:red,stroke-width:2px;
    style equip fill:#eef,stroke:#333,stroke-width:2px;
    style master fill:#fee,stroke:#333,stroke-width:2px;
    
    %% Ссылки
    click p4 href "https://iu5.bmstu.ru" "переход для Мастера" _blank;
    click p5 href "https://mermaid.js.org/syntax/flowchart.html" "параметры ремонта" _blank;
```
