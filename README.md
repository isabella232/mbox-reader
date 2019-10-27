# MboxReader

Reads messages from a MBOX file. Output is an async generator where messages are yielded one by one.

MboxReader supports very large (multi gigabyte) mbox files as messages are processed asynchronously.

**NB!** _mboxcl2_ formatted mbox files are not supported as MboxReader assumes From-munging.

## Usage

```
eachMessage(readableStream) -> async generator
```

Generator yields objects with the following properties:

-   **returnPath** Sender email address
-   **time** Sending time as a Date object or `false` if date was not parseable
-   **content** is a Buffer with the RFC822 formatted message
-   **flags** a Set of IMAP compatible flags derived from Status and X-Status headers
-   **labels** a Set of Gmail labels from X-Gmail-Labels headers
-   **headers** a Map of headers where key is lowercase header key and value is an array of header lines for this key (without the key prefix)

**Example**

Example reads mbox file from standard input and writes messages to console.

```
const { eachMessage } = require('mbox-reader');

for await (let message of eachMessage(process.stdin)) {
    console.log(message.returnPath);
    console.log(message.time);
    process.stdout.write(message.content);
}
```

## License

ISC
