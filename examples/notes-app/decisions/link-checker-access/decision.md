## LinkCheckerAccess

### Context

`CheckNoteLinks` is delegated to the Link Checker spec, whose own `DocumentTransport` decision requires that documents arrive over HTTP.

### Requirement

The application must be able to submit a note's body as a `Document` and receive a `DocumentReport`, while honoring the delegated spec's decisions.

### Decision

The application bundles a link checker instance and calls it over HTTP on the local machine, as the delegated spec requires. Link checking is unavailable while that instance is not running.
