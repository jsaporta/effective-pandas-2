## Prompt

Please provide a comprehensive list of [pyarrow] types that can be used in Pandas 2.x.

## ChatGPT 5 Response

Here’s the **complete set of PyArrow-backed dtypes you can use in Pandas 2.x**, with the canonical dtype strings (where available) and notes on parameterized types.

### Numeric

* `bool[pyarrow]` (nullable boolean). ([Pandas][1])
* `int8[pyarrow]`, `int16[pyarrow]`, `int32[pyarrow]`, `int64[pyarrow]`. ([Pandas][1])
* `uint8[pyarrow]`, `uint16[pyarrow]`, `uint32[pyarrow]`, `uint64[pyarrow]`. ([Pandas][1])
* `float32[pyarrow]`, `float64[pyarrow]`. ([Pandas][1])

### Strings & Binary

* `string[pyarrow]` (Arrow-backed string). Note: the alias maps to `pd.StringDtype("pyarrow")`, which is distinct from `pd.ArrowDtype(pa.string())`. ([Pandas][2])
* `binary[pyarrow]` (Arrow binary; operations are limited in pandas). (Listed in the pandas/Arrow type table.) ([Pandas][1])

### Temporal

* **Timestamp (with optional tz & unit):** use `pd.ArrowDtype(pa.timestamp(unit, tz))` (e.g., `timestamp[ns][pyarrow]`, `timestamp[us, tz=UTC][pyarrow]`). In pandas this maps to the timezone-aware datetime extension. ([Pandas][1])
* **Time of day:** `time32[...]` / `time64[...]` via `pd.ArrowDtype(pa.time64("us"))`, yielding e.g. `time64[us][pyarrow]`. ([Pandas][2])
* **Date:** `date32[pyarrow]`, `date64[pyarrow]` (Arrow date types recognized by pandas). ([Pandas][1])
* **Duration/Timedelta:** `duration[...]` via `pd.ArrowDtype(pa.duration("ns"|"us"|"ms"|"s"))` (recognized; limited pandas-native ops). ([Pandas][1])

### Decimal (fixed-point)

* **Decimal:** `decimal128(precision, scale)[pyarrow]` and `decimal256(precision, scale)[pyarrow]` via `pd.ArrowDtype(pa.decimal128(...))` / `pa.decimal256(...)`. ([Pandas][2])

### Nested / Complex

* **List:** `list<item: T>[pyarrow]` via `pd.ArrowDtype(pa.list_(pa.string()), …)`. ([Pandas][2])
* **Map:** `map<key: K, value: V>[pyarrow]` (construct with a `pyarrow.Array`/`ChunkedArray` and wrap in `pd.arrays.ArrowExtensionArray`). ([Pandas][2])
* **Dictionary-encoded (categorical):** Arrow `dictionary()` maps to pandas `CategoricalDtype` (i.e., `category` column backed by Arrow). ([Pandas][1])

---

#### How to specify them

* For **simple scalar types**, you can pass the **string alias** directly: e.g. `dtype="int64[pyarrow]"`, `dtype="bool[pyarrow]"`, `dtype="string[pyarrow]"`. ([Pandas][2])
* For **parameterized types** (timestamps with units/tz, time, duration, decimal, list, map), construct with **`pd.ArrowDtype(pa.<type>(...))`**, e.g.:

  * `pd.ArrowDtype(pa.timestamp("ns", tz="UTC"))`
  * `pd.ArrowDtype(pa.time64("us"))`
  * `pd.ArrowDtype(pa.decimal128(10, scale=2))`
  * `pd.ArrowDtype(pa.list_(pa.string()))` ([Pandas][2])

> Note: Arrow-backed dtypes in pandas are implemented via `ArrowDtype`/`ArrowExtensionArray` and are still marked **experimental** in the 2.x series (APIs may change). ([Pandas][3])

If you want, I can also give you a quick code snippet that constructs a DataFrame showing each of these dtypes in action.

[1]: https://pandas.pydata.org/docs/reference/arrays.html "pandas arrays, scalars, and data types — pandas 2.3.2 documentation"
[2]: https://pandas.pydata.org/docs/user_guide/pyarrow.html "PyArrow Functionality — pandas 2.3.2 documentation"
[3]: https://pandas.pydata.org/docs/reference/api/pandas.ArrowDtype.html?utm_source=chatgpt.com "pandas.ArrowDtype — pandas 2.3.2 documentation - PyData |"
