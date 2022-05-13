# Merge transition tests

All test cases described in this document are beginning in a pre-Merge world and reach the terminal PoW during execution to perform the PoS transition.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [EL client tests](#el-client-tests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## EL client tests

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L103)] Re-org to Higher-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `B1'.totalDifficulty > B1.totalDifficulty`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1' <- P1`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Post-Merge Re-org to Higher-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `B1'.totalDifficulty > B1.totalDifficulty`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1 <- P1`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1')` + `forkchoiceUpdated(head: P1')`, where `B1' <- P1'`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn', validationError: null}` to `forkchoiceUpdated(head: Pn')` where `B1' <- P1' <- ... <- Pn'`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L115)] Re-org to Lower-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `B1.totalDifficulty > B1'.totalDifficulty`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1' <- P1`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Post-Merge Re-org to Lower-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `B1.totalDifficulty > B1'.totalDifficulty`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1 <- P1`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1')` + `forkchoiceUpdated(head: P1')`, where `B1' <- P1'`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn', validationError: null}` to `forkchoiceUpdated(head: Pn')` where `B1' <- P1' <- ... <- Pn'`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L127)] Two-Block Re-org to Higher-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1' <- B2'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B2.totalDifficulty > TTD`, `B2'.totalDifficulty > TTD`
  * `B2'.totalDifficulty > B2.totalDifficulty`
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
  * `forkchoiceUpdated(head: B2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B2' <- P1`
    * EL eventually syncs `B1'`, `B2'`, their descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- B2' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L139)] Two-Block Re-org to Lower-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1' <- B2'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B2.totalDifficulty > TTD`, `B2'.totalDifficulty > TTD`
  * `B2.totalDifficulty > B2'.totalDifficulty`
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
  * `forkchoiceUpdated(head: B2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B2' <- P1`
    * EL eventually syncs `B1'`, `B2'`, their descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- B2' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L151)] Re-org to Higher-Height PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1' <- B2'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B2'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `forkchoiceUpdated(head: B2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B2' <- P1`
    * EL eventually syncs `B1'`, `B2'`, their descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- B2' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Post-Merge Re-org to Higher-Height PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1' <- B2'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B1.totalDifficulty > TTD`, `B2'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1 <- P1`
  * `forkchoiceUpdated(head: B2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1')` + `forkchoiceUpdated(head: P1')`, where `B2' <- P1'`
    * EL eventually syncs `B1'`, `B2'`, their descendants, and responds `{status: VALID, latestValidHash: Pn', validationError: null}` to `forkchoiceUpdated(head: Pn')` where `B1' <- B2' <- P1' <- ... <- Pn'`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L163)] Re-org to Lower-Height PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B2.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1' <- P1`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Post-Merge Re-org to Lower-Height PoW Chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B2.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B2 <- P1`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1')` + `forkchoiceUpdated(head: P1')`, where `B1' <- P1'`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn', validationError: null}` to `forkchoiceUpdated(head: Pn')` where `B1' <- P1' <- ... <- Pn'`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Re-org to Chain With Invalid Terminal Block (`Block.totalDifficulty < TTD`)
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * EL client 1 has higher `TTD` configuration than EL client 2, such that:
    * For EL client 1, `B2.totalDifficulty > TTD`, `B1.totalDifficulty < TTD`, `B1'.totalDifficulty < TTD`
    * For EL client 2, `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1' <- P1`
    * EL eventually syncs `B1'`, its descendants, but `head` remains `B2`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/537)] Re-org to Chain With Invalid Terminal Block (`Block.Parent.totalDifficulty > TTD`)
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1`, `B: Genesis <- B1' <- B2'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * EL client 1 has lower `TTD` configuration than EL client 2, such that:
    * For EL client 1, `B1.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
    * For EL client 2, `B1.totalDifficulty < TTD`, `B1'.totalDifficulty < TTD`, `B2'.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B1)`
    * EL responds with `{status: VALID, latestValidHash: B1, validationError: null}`
  * `forkchoiceUpdated(head: B2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B2' <- P1`
    * EL eventually syncs `B1'`, `B2'`, its descendants, but `head` remains `B1`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/538)] Re-org to Lower-Height PoW Chain with `PREVRANDAO` opcode transaction
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- B1 <- B2`, `B: Genesis <- B1'`
  * EL client 1 starts with `A`, EL client 2 starts with `B`
  * Both clients have the same `TTD` configuration
  * `B2.totalDifficulty > TTD`, `B1'.totalDifficulty > TTD`
  * `B1` and `B1'` contain `Tx1`
  * `B2` contains `Tx2`
  * `Tx1` and `Tx2` save `DIFFICULTY` opcode to storage
  * `forkchoiceUpdated(head: B2)`
    * EL responds with `{status: VALID, latestValidHash: B2, validationError: null}`
    * Storage matches the `DIFFICULTY` opcode
  * `forkchoiceUpdated(head: B1')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1' <- P1`
    * `P1` contains `Tx2`
    * EL eventually syncs `B1'`, its descendants, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1' <- P1 <- ... <- Pn`
    * Storage matches the `PREVRANDAO` opcode
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L175)] Halt following PoW chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- B1 <- B2`
  * EL client 1 starts with chain up to block `B1`, EL client 2 starts with chain up to block `B2`
  * EL client 2 has a higher `TTD` configuration than EL client 1
  * `forkchoiceUpdated` is not sent to EL client 1
  * Wait two minutes
    * EL client 1 does not incorporate `B2` into its chain
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L191)] Long PoW Chain Sync
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- B1 <- ... <- B1024`
  * EL client 1 starts with chain up to block `B1`, EL client 2 starts with chain up to block `B1024`
  * Both clients have the same `TTD` configuration
  * `B1024.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B1024)`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1024 <- P1`
    * EL eventually syncs `B1 <- ... <- B1024`, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1 <- ... <- B1024 <- P1 <- ... <- Pn`
  
  </details>
