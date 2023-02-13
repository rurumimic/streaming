# Apache Beam

- [beam](https://beam.apache.org/)
  - go: [examples](https://github.com/apache/beam/tree/master/sdks/go/examples)
- [get started](https://beam.apache.org/get-started/)
- [documentation](https://beam.apache.org/documentation/)

## WordCount

### Get the SDK and the examples

example: [wordcount.go](https://github.com/apache/beam/blob/master/sdks/go/examples/wordcount/wordcount.go)

```bash
go mod init wordcount
go get -u github.com/apache/beam/sdks/v2/go/pkg/beam
go mod tidy
```

### Run wordcount

google cloude storage: [shakespeare/kinglear.txt](https://github.com/apache/beam/blob/master/sdks/go/data/shakespeare/kinglear.txt)

```bash
go run . --output counts
```

<details>
  <summary>Logs</summary>

```log
2023/02/13 13:46:07 Executing pipeline with the direct runner.
2023/02/13 13:46:07 Pipeline:
2023/02/13 13:46:07 Nodes: {1: []uint8/bytes GLO}
{2: string/string GLO}
{3: string/string GLO}
{4: KV<string,int64>/KV<string,varint> GLO}
{5: string/string GLO}
{6: string/string GLO}
{7: KV<string,int>/KV<string,int[varintz]> GLO}
{8: CoGBK<string,int>/CoGBK<string,int[varintz]> GLO}
{9: KV<string,int>/KV<string,int[varintz]> GLO}
{10: string/string GLO}
{11: KV<int,string>/KV<int[varintz],string> GLO}
{12: CoGBK<int,string>/CoGBK<int[varintz],string> GLO}
Edges: 1: Impulse [] -> [Out: []uint8 -> {1: []uint8/bytes GLO}]
2: ParDo [In(Main): []uint8 <- {1: []uint8/bytes GLO}] -> [Out: T -> {2: string/string GLO}]
3: ParDo [In(Main): string <- {2: string/string GLO}] -> [Out: string -> {3: string/string GLO}]
4: ParDo [In(Main): string <- {3: string/string GLO}] -> [Out: KV<string,int64> -> {4: KV<string,int64>/KV<string,varint> GLO}]
5: ParDo [In(Main): KV<string,int64> <- {4: KV<string,int64>/KV<string,varint> GLO}] -> [Out: string -> {5: string/string GLO}]
6: ParDo [In(Main): string <- {5: string/string GLO}] -> [Out: string -> {6: string/string GLO}]
7: ParDo [In(Main): T <- {6: string/string GLO}] -> [Out: KV<T,int> -> {7: KV<string,int>/KV<string,int[varintz]> GLO}]
8: CoGBK [In(Main): KV<string,int> <- {7: KV<string,int>/KV<string,int[varintz]> GLO}] -> [Out: CoGBK<string,int> -> {8: CoGBK<string,int>/CoGBK<string,int[varintz]> GLO}]
9: Combine [In(Main): int <- {8: CoGBK<string,int>/CoGBK<string,int[varintz]> GLO}] -> [Out: KV<string,int> -> {9: KV<string,int>/KV<string,int[varintz]> GLO}]
10: ParDo [In(Main): KV<string,int> <- {9: KV<string,int>/KV<string,int[varintz]> GLO}] -> [Out: string -> {10: string/string GLO}]
11: ParDo [In(Main): T <- {10: string/string GLO}] -> [Out: KV<int,T> -> {11: KV<int,string>/KV<int[varintz],string> GLO}]
12: CoGBK [In(Main): KV<int,string> <- {11: KV<int,string>/KV<int[varintz],string> GLO}] -> [Out: CoGBK<int,string> -> {12: CoGBK<int,string>/CoGBK<int[varintz],string> GLO}]
13: ParDo [In(Main): CoGBK<int,string> <- {12: CoGBK<int,string>/CoGBK<int[varintz],string> GLO}] -> []
2023/02/13 13:46:07 Plan[plan]:
15: Impulse[0]
1: ParDo[textio.writeFileFn] Out:[] Sig: func(context.Context, int, func(*string) bool) error
2: CoGBK. Out:1
3: Inject[0]. Out:2
4: ParDo[beam.addFixedKeyFn] Out:[3] Sig: func(typex.T) (int, typex.T)
5: ParDo[main.formatFn] Out:[4] Sig: func(string, int) string
6: Combine[stats.sumIntFn] Keyed:false Out:5
7: CoGBK. Out:6
8: Inject[0]. Out:7
9: ParDo[stats.keyedCountFn] Out:[8] Sig: func(typex.T) (typex.T, int)
10: ParDo[main.extractFn] Out:[9] Sig: func(context.Context, string, func(string))
11: SDF.SdfFallback[textio.readFn] UID:11 Out:[10]
12: ParDo[textio.sizeFn] Out:[11] Sig: func(context.Context, string) (string, int64, error)
13: ParDo[textio.expandFn] Out:[12] Sig: func(context.Context, string, func(string)) error
14: ParDo[beam.createFn] Out:[13] Sig: func([]uint8, func(typex.T)) error
2023/02/13 13:46:07 Warning: falling back to unauthenticated GCS access: dialing: google: could not find default credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.
2023/02/13 13:46:07 Warning: falling back to unauthenticated GCS access: dialing: google: could not find default credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.
2023/02/13 13:46:07 Reading from gs://apache-beam-samples/shakespeare/kinglear.txt
2023/02/13 13:46:07 Warning: falling back to unauthenticated GCS access: dialing: google: could not find default credentials. See https://developers.google.com/accounts/docs/application-default-credentials for more information.
2023/02/13 13:46:08 Writing to counts
```

</details>

#### Counts Result

```bash
head counts
```

```bash
taking: 7
Strives: 1
vices: 3
convey'd: 1
waste: 1
aidant: 1
briefness: 1
flakes: 1
second: 1
Looking: 1
```
