# Appendix C — ComputeSubgraphContract

## C.1. Назначение

Этот appendix фиксирует минимальную формализацию контракта подграфа и алгоритма `ComputeSubgraphContract`, используемых для декомпозиции, агрегации и локальной верификации структурной реализации.

## C.2. Предпосылки

Пусть структурная реализация задана как типизированный ориентированный мультиграф

`G = (V, E, src, dst, τ_V, τ_E)`,

где:
- `τ_V(v) ∈ {function, table}`;
- `τ_E(e) ∈ {consumes, produces, reads, writes, triggers}`.

Каждая функция `f` имеет контракт

`C(f) = ⟨Cin, Cout, R, W, Tin, Tout⟩`,

где компоненты полностью выводятся из инцидентных рёбер соответствующего типа.

## C.3. Контракт подграфа

Для выбранного подграфа `S` множество его вершин обозначается как `V(S)` и раскладывается на две части:

`V(S) = V_func(S) ∪ V_table(S)`,

где `V_func(S)` — множество вершин типа `function`, принадлежащих `S`, а `V_table(S)` — множество вершин типа `table`, принадлежащих `S`.

`E(S) = { e ∈ E(G) | src(e) ∈ V(S) ∨ dst(e) ∈ V(S) }` — множество рёбер графа `G`, у которых хотя бы один конец принадлежит `V(S)`.

Для подграфа `S` определяется контракт

`C(S) = ⟨Cin(S), Cout(S), R(S), W(S), Tin(S), Tout(S)⟩`.

Он описывает только внешнюю границу подграфа: какие таблицы и функции соединяют `S` с остальной частью графа.

## C.4. Алгоритм `ComputeSubgraphContract`

```text
function ComputeSubgraphContract(S):
    V_func(S)  = {v ∈ V(S) : τ_V(v) = function}
    V_table(S) = {v ∈ V(S) : τ_V(v) = table}

    internal_tables = {t ∈ V_table(S) :
        ∀e ∈ E where (src(e)=t or dst(e)=t) :
            (src(e) ∈ V(S) and dst(e) ∈ V(S))}

    interface_tables = V_table(S) \ internal_tables

    Cin(S)  = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = consumes,
               src(e) = t, dst(e) ∈ V_func(S)}

    Cout(S) = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = produces,
               src(e) ∈ V_func(S), dst(e) = t}

    R(S)    = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = reads,
               src(e) = t, dst(e) ∈ V_func(S)}

    W(S)    = {t ∈ interface_tables : ∃e ∈ E(S), τ_E(e) = writes,
               src(e) ∈ V_func(S), dst(e) = t}

    Tin(S)  = {f ∉ V(S) : ∃e ∈ E, τ_E(e) = triggers,
               src(e) = f, dst(e) ∈ V_func(S)}

    Tout(S) = {f ∉ V(S) : ∃e ∈ E, τ_E(e) = triggers,
               src(e) ∈ V_func(S), dst(e) = f}

    return ⟨Cin(S), Cout(S), R(S), W(S), Tin(S), Tout(S)⟩
```

## C.5. Примечание об interface tables

Интерфейсная таблица может одновременно входить в несколько компонент контракта подграфа. Например, таблица может потребляться одной граничной функцией и порождаться другой. Это штатный случай: контракт подграфа фиксирует внешние связи, а не навязывает таблице единственную роль.

## C.6. Использование

### Декомпозиция

При декомпозиции узел `f` заменяется подграфом `S` так, чтобы выполнялось:

`C(f) = C(S)`.

Промежуточные внутренние таблицы допускаются, если они не выходят на границу подграфа.

### Агрегация

При агрегации подграф `S` сворачивается в единый узел `f_S` с контрактом

`C(f_S) = C(S)`.

Это делает `ComputeSubgraphContract` минимальным формальным механизмом, обеспечивающим contract-preserving преобразования между разными уровнями детализации одной и той же структурной реализации.
