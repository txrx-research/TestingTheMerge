# Merge transition tests

All test cases described in this document are beginning in a pre-Merge world and reach the terminal PoW during execution to perform the PoS transition.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [`TERMINAL_TOTAL_DIFFICULTY`](#terminal_total_difficulty)
  - [EL client tests](#el-client-tests)
  - [CL client tests](#cl-client-tests)
- [[BONUS] `TERMINAL_BLOCK_HASH`](#bonus-terminal_block_hash)
  - [EL client tests](#el-client-tests-1)
  - [CL client tests](#cl-client-tests-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## `TERMINAL_TOTAL_DIFFICULTY`

### EL client tests

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L103)] Re-org to Higher-Total-Difficulty PoW Chain
  <details>
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

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
  <summary>Click for details</summary>

  * `Genesis <- B1 <- B2`
  * EL client 1 starts with chain up to block `B1`, EL client 2 starts with chain up to block `B2`
  * EL client 2 has a higher `TTD` configuration than EL client 1
  * `forkchoiceUpdated` is not sent to EL client 1
  * Wait two minutes
    * EL client 1 does not incorporate `B2` into its chain
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/mergetests.go#L191)] Long PoW Chain Sync
  <details>
  <summary>Click for details</summary>

  * `Genesis <- B1 <- ... <- B1024`
  * EL client 1 starts with chain up to block `B1`, EL client 2 starts with chain up to block `B1024`
  * Both clients have the same `TTD` configuration
  * `B1024.totalDifficulty > TTD`
  * `forkchoiceUpdated(head: B1024)`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * `newPayload(P1)` + `forkchoiceUpdated(head: P1)`, where `B1024 <- P1`
    * EL eventually syncs `B1 <- ... <- B1024`, and responds `{status: VALID, latestValidHash: Pn, validationError: null}` to `forkchoiceUpdated(head: Pn)` where `B1 <- ... <- B1024 <- P1 <- ... <- Pn`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/32ff59c89c879972f00abe62137a566def81a080/simulators/ethereum/engine/clmock.go#L346)] Propose valid transition block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- TB`, `TB` is a valid terminal block
  * EL starts with `TB` as the head
  * `forkchoiceUpdated(headBlockHash: TB.blockHash, payloadAttributes: mergeTransitionBlockAttributes)`
    * EL's head is set to `headBlockHash`
    * EL returns `{status: VALID, payloadId: mergeTransitionPayloadId}`
  * `getPayload(mergeTransitionPayloadId)`
    * EL returns merge transition payload
      * `ommersHash == 0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347`
      * `difficulty == 0`
      * `mixHash == mergeTransitionBlockAttributes.prevRandao`
      * `nonce == 0x0000000000000000`
  * `newPayload(mergeTransitionPayload)`
    * EL returns `VALID`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/553)] Transition on a valid chain with missed `fcU`
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- TB <- P1 <- P2 <- P3`, `TB` is a valid terminal block
  * EL starts with `TB` as the head
  * `newPayload(P1)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `TB`
  * `newPayload(P2)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `TB`
  * `newPayload(P3)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `TB`
  * `forkchoiceUpdated(head: P3, safe: P2, finalized: P1)`
    * EL's head updated to `P3`
    * `eth_getBlockByNumber(safe)` returns `P2`
    * `eth_getBlockByNumber(finalized)` returns `P1`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/f2838a629dcd1a027a260cd10fc84899f89e3ba6/simulators/ethereum/engine/enginetests.go#L410)] Build atop of invalid terminal block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- INV_TB`, `INV_TB` is a *valid PoW* but an *invalid terminal* block, i.e. `INV_TB.TD < TTD`; `INV_TB.parent.TD >= TTD` can't be checked easily as EL wouldn't process such block
  * EL starts with `INV_TB` as the head
  * `forkchoiceUpdated(headBlockHash: TB.blockHash, payloadAttributes: mergeTransitionBlockAttributes)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00, payloadId: null}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/552)] Transition on a valid chain
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- TB <- P1 <- P2 <- P3`, `TB` is a valid terminal block
  * EL starts with `TB` as the head
  * `newPayload(P1)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `TB`
  * `forkchoiceUpdated(head: P1, safe: 0x00..00, finalized: 0x00..00)`
    * EL returns `{status: VALID, latestValidHash: forkchoiceState.headBlockHash}`
    * EL's head updated to `P1`
    * `eth_getBlockByNumber(safe)` returns `-39001: Unknown block` error
    * `eth_getBlockByNumber(finalized)` returns `-39001: Unknown block` error
  * `newPayload(P2) + forkchoiceUpdated(head: P2, safe: P1, finalized: 0x00..00)`
    * EL's head updated to `P2`
    * `eth_getBlockByNumber(safe)` returns `-39001: Unknown block` error
    * `eth_getBlockByNumber(finalized)` returns `-39001: Unknown block` error
  * `newPayload(P3) + forkchoiceUpdated(head: P3, safe: P2, finalized: P1)`
    * EL's head updated to `P3`
    * `eth_getBlockByNumber(safe)` returns `P2`
    * `eth_getBlockByNumber(finalized)` returns `P1`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/554)] Transition on an invalid chain
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- INV_TB <- P1`, `INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [ ] `INV_TB.TD < TTD`
    * [ ] `INV_TB.parent.TD >= TTD` -- it might require a second EL with a higher `TTD` value
  * EL starts with `INV_TB` as the head
  * `newPayload(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/556)] Re-org to a chain with invalid transition block
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2`, `B: Genesis <- ... <- TB <- INV_P1`, `A.TB` and `B.TB` are valid terminal blocks
  * EL starts with `A.TB` as the head
  * `newPayload(A.P1) + newPayload(A.P2)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `A.TB`
  * `forkchoiceUpdated(head: A.P2, safe: A.P1, finalized: 0x00..00)`
    * EL's head updated to `A.P2`
  * `newPayload(B.INV_P1)`
    * EL returns `{status: INVALID/ACCEPTED, latestValidHash: null/0x00..00}`
    * EL's head points to `A.P2`
  * `forkchoiceUpdated(head: B.INV_P1, safe: 0x00..00, finalized: 0x00..00)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `A.P2`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/f2838a629dcd1a027a260cd10fc84899f89e3ba6/simulators/ethereum/engine/mergetests.go#L244)] Syncing with the chain having a valid transition
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn <- TB <- P1`, `TB` is a valid terminal block
  * EL starts with `Bn` as the head
  * `newPayload(P1) + forkchoiceUpdated(P1)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * EL should pull `TB <- P1` from the network
  * poll `forkchoiceUpdated(P1)`
    * EL returns `{status: VALID, latestValidHash: null}`
    * EL's head points to `P1`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/627)] Syncing with the chain having invalid *terminal* block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn <- INV_TB <- P1 <- P2`, `INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [x] [[Hive](https://github.com/ethereum/hive/pull/627)] `INV_TB.TD < TTD`
    * [x] [[Hive](https://github.com/ethereum/hive/pull/627)] `INV_TB.parent.TD >= TTD` -- it might require a second EL with a higher `TTD` value
  * EL starts with `Bn` as the head
  * `newPayload(P1) + forkchoiceUpdated(P1)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * EL should pull `INV_TB <- P1` from the network
  * poll `forkchoiceUpdated(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `Bn`
  
  </details>

* [ ] Syncing with the chain where *terminal* block is invalid with respect to EE
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn <- INV_TB <- P1 <- P2`, `INV_TB` satisfies `TTD` but is invalid in terms of execution
  * EL starts with `Bn` as the head
  * `newPayload(P1) + forkchoiceUpdated(P1)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * EL should pull `INV_TB <- P1` from the network
  * poll `forkchoiceUpdated(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `Bn`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/627)] Syncing with the chain having invalid *transition* block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn <- TB <- INV_P1 <- P2`, `TB` is a valid terminal block, `INV_P1` is an invalid payload
  * EL starts with `Bn` as the head
  * `newPayload(P2) + forkchoiceUpdated(P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * poll `forkchoiceUpdated(P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `TB`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/627)] Syncing with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn <- TB <- P1 <- INV_P2 <- P3`, `TB` is a valid terminal block, `INV_P2` is an invalid payload
  * EL starts with `Bn` as the head
  * `newPayload(P3) + forkchoiceUpdated(P3)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * poll `forkchoiceUpdated(P3)`
    * EL returns `{status: INVALID, latestValidHash: P1.blockHash}`
    * EL's head points to `TB` (in this case `fcU` won't be applied at all because it does point to a block on an invalid chain, thus, no update and PoS transition should happen; normally, a CL will send `fcU(P1)` when get `lvh = P1.blockHash`)
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/4500c1ee28133a8090d126da789d13e11b7ef3f0/simulators/ethereum/engine/suites/transition/tests.go#L338-L387)] Re-org and sync with the chain having invalid *terminal* block
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2`, `B: Genesis <- ...  <- INV_TB <- P1 <- P2`, `B.INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [[Hive](https://github.com/ethereum/hive/blob/4500c1ee28133a8090d126da789d13e11b7ef3f0/simulators/ethereum/engine/suites/transition/tests.go#L372)] `INV_TB.TD < TTD`
    * [[Hive](https://github.com/ethereum/hive/blob/4500c1ee28133a8090d126da789d13e11b7ef3f0/simulators/ethereum/engine/suites/transition/tests.go#L338)] `INV_TB.parent.TD >= TTD`
  * This scenario may require second EL with a higher `TTD` value
  * EL starts with `A.P2` as the head
  * `newPayload(B.P2) + forkchoiceUpdated(P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P2`
  * EL should pull `B: INV_TB <- P1 <- P2` from the network
  * poll `forkchoiceUpdated(B.P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `A.P2`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/627)] Re-org and sync with the chain having invalid *transition* block
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2`, `B: Genesis <- ... <- TB <- INV_P1 <- P2`, `B.TB` is a valid terminal block, `B.INV_P1` is an invalid payload
  * EL starts with `A.P2` as the head
  * `newPayload(B.P2) + forkchoiceUpdated(B.P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P2`
  * poll `forkchoiceUpdated(B.P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `A.P2`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/master/simulators/ethereum/engine/suites/engine/tests.go#L315-L405)] Re-org and sync with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2 <- P3`, `B: Genesis <- ... <- TB <- P1 <- INV_P2 <- P3`, `B.TB` is a valid terminal block, `B.INV_P2` is an invalid payload
  * EL starts with `A.P2` as the head
  * `newPayload(B.P3) + forkchoiceUpdated(B.P3)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P3`
  * poll `forkchoiceUpdated(B.P3)`
    * EL returns `{status: INVALID, latestValidHash: B.P1.blockHash}`
    * EL's head points to `A.P3` (in this case `fcU` won't be applied at all because it does point to a block on an invalid chain, thus, no update of the forkchoice state should happen; normally, a CL will send `fcU(B.P1)` when get `lvh = B.P1.blockHash` and switch canonical chain to `B` with `B.P1` as the head)
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/613)] Stop processing gossiped PoW blocks
  <details>
  <summary>Click for details</summary>

  * PoW/Clique miner builds a chain and advertises it via gossip; the chain goes beyond a terminal PoW block
  * EL is connected to the miner and does not process blocks after the terminal one, i.e. the head points to the terminal block
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/f2838a629dcd1a027a260cd10fc84899f89e3ba6/simulators/ethereum/engine/mergetests.go#L228)] Stop processing synced PoW blocks
  <details>
  <summary>Click for details</summary>

  * PoW client starts with `Genesis <- ... <- TB <- B1 <- B2 <- ... <- Bn` chain, where `TB` is a valid terminal block
  * EL connects to the PoW client and syncs with the advertised chain
    * EL's head points to `TB`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/613)] Terminal blocks are gossiped
  <details>
  <summary>Click for details</summary>

  * Consider `PoW <-> EL1 <-> EL2` as a PoW client connected to EL client `EL1`, which in its turn connected to EL client `EL2`. There is no direct connection between `EL2` and `PoW` clients.
  * `PoW` client gossips a terminal block `TB`
    * Head of `EL2` client points to `TB`
  * `PoW` client gossips a descendant of `TB`
    * Head of `EL2` client points to `TB`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/613)] Build Payload After Multiple Terminal blocks via gossip
  <details>
  <summary>Click for details</summary>

  * Consider `PoW <-> EL` as a PoW client connected to EL client `EL`.
  * `PoW` client produces and gossips multiple terminal blocks `TB1`, `TB2`, ... `TBN`
  * `newPayload(TBN)` is sent to `EL`
    * `EL` immediately returns `{status: VALID, latestValidHash: TBN.BlockHash}`
  
  </details>

### CL client tests

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Transition on a valid chain
  <details>
  <summary>Click for details</summary>

  * `EL: Genesis <- ... <- TB`, `TB` is a valid terminal block
  * `CL: Genesis <- ... <- Bellatrix`
  * EL starts with `TB` as the head or mines a chain up to `TB` 
  * CL starts with `Genesis` and builds a chain up to `Bellatrix` and upgrades to `Bellatrix`
  * CL drives EL through transition and finalizes it
    * `eth_getBlockByNumber(latest)` returns the head
    * `eth_getBlockByNumber(safe)` returns the most recent justified block
    * `eth_getBlockByNumber(finalized)` returns the most recent finalized block
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Build atop of invalid terminal block
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- Bn`, `Bn` is a *valid PoW* but an *invalid terminal* block, i.e. `Bn.TD < EL.TTD`
  * `Bn` is a valid terminal block to CL's observation, i.e. CL's `TERMINAL_TOTAL_DIFFICULTY` is configured in the following way:
    * `Bn.TD >= CL.TTD && Bn.parent.TD < CL.TTD`
  * EL starts with `Bn` as the head
  * CL starts with `Genesis` and build a chain up to Bellatrix
    * Transition never happens, i.e. CL's head block always contains zeroed payload (`ExecutionPayload()`)
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/01ce64b0354c5551956a98e817328e0df43d61c7/simulators/eth2/engine/scenarios.go#L409)] Transition on a chain with invalid *terminal* block
  <details>
  <summary>Click for details</summary>

  * `EL: Genesis <- ... <- Bn`
  * Nodes: builder, importer; importer is connected to builder on both layers
  * `Bn` is a valid terminal block to the builder's observation, i.e. builder's `TERMINAL_TOTAL_DIFFICULTY` is configured in the following way:
    * `Bn.TD >= TTD && Bn.parent.TD < TTD`
  * `Bn` is an *invalid* terminal block to the importer's observation, i.e. builder's `TERMINAL_TOTAL_DIFFICULTY` is configured in the following way:
    * `Bn.TD < TTD`
  * EL starts with `Bn` as the head
  * The builder starts with `Genesis` and build a chain up to Bellatrix and beyond
    * Importer does never imports a transition block and its head does always point to pre-transition block
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/01ce64b0354c5551956a98e817328e0df43d61c7/simulators/eth2/engine/scenarios.go#L132)] Transition on a chain with invalid *transition* block
  <details>
  <summary>Click for details</summary>

  * `EL: Genesis <- ... <- Bn`
  * Nodes: builder, importer; importer is connected to builder on both layers
  * `Bn` is a valid terminal block to builder and importer observation
  * EL starts with `Bn` as the head
  * CL starts with `Genesis` and the builder builds a chain up to Bellatrix and beyond
  * EL mock of the importer node returns `INVALID` to the transition block's payload
    * Importer does never imports a transition block and its head does always point to pre-transition block
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Syncing with the chain having a valid transition block
  <details>
  <summary>Click for details</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: builder, importer; builder starts synced with `EL.A`, importer with `EL.B`, same `TD` value on both chains prevents importer from pulling builder's chain beforehand
  * The builder starts with `CL: Genesis` and builds a chain up to Bellatrix and beyond
    * Importer will have to pull `EL.A` from builder
    * Importer block processing is paused and waits for [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) to proceed -- this might worth checking
    * After the timeout exceedes importer eventually syncs and passes through the transition
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Syncing with the chain having invalid *transition* block
  <details>
  <summary>Click for details</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: builder, importer; builder starts synced with `EL.A`, importer with `EL.B`, same `TD` value on both chains prevents importer from pulling builder's chain beforehand
  * `A.Bn` is a valid terminal block to the builder's observation
  * `A.Bn` may be an *invalid* terminal block to the importer's observation, but it's not necessary as EL mock will emulate this invalidity
  * The builder starts with `CL: Genesis` and builds a chain up to Bellatrix and beyond
    * Importer will have to pull `EL.A` from builder
    * Importer block processing is paused and waits for [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) to proceed -- may not be checked in this scenario
    * Importer's EL mock artificially keeps returning `SYNCING` to let importer's CL reach the point farther away from transition block
    * After some time EL mock starts to respond with `{status: INVALID, latestValidHash: 0x00..00}` to every `newPayload` and `forkchoiceUpdated` call -- this would happen in real life if either terminal or transition block would be invalid
    * The importer must invalidate blocks starting from a transition one and must never import a transition block again
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Syncing with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: builder, importer; builder starts synced with `EL.A`, importer with `EL.B`, same `TD` value on both chains prevents importer from pulling builder's chain beforehand
  * `A.Bn` is a valid terminal block to the builder's and importer's observation
  * The builder starts with `CL: Genesis` and builds a chain up to Bellatrix and beyond
    * Importer will have to pull `EL.A` from builder
    * Importer block processing is paused and waits for [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) to proceed -- may not be checked in this scenario
    * Importer's EL mock artificially keeps returning `SYNCING` to let importer's CL reach the point farther away from *post-transition* block
    * After some time EL mock starts to respond with `{status: INVALID, latestValidHash: transitionBlock.blockHash}` to every `newPayload` and `forkchoiceUpdated` call -- this would happen in real life if either terminal or transition block would be invalid
    * The importer must invalidate blocks starting from a post-transition one and must never import a post-transition block again, and the head must point to a transition block
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/606)] Re-org and sync with the chain having invalid *terminal* block
  <details>
  <summary>Click for details</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: valid builder, invalid builder, importer; valid builder and importer start synced with `EL.A`, invalid builder with `EL.B`, same `TD` value on both chains prevents importer and valid builder from pulling invalid builder's chain beforehand. Valid builder has `1/2n - x`, invalid builder has `1/2 + x` validators, where `x` is a small number
  * `A.Bn` is a valid terminal block to valid builder's and importer's observation
  * `B.Bn` is an *invalid* terminal block to the importer's observation, but valid to the invalid builder's observation
  * Two builders start with `CL: Genesis` and builds a chain up to Bellatrix and beyond. They should stay in consensus up to Bellatrix and then fall apart starting with a transition block
    * Importer's head eventually is coherent with a valid builder
    * Ideally, the valid builder proposes a transition block *before* the invalid builder's does the same -- these can be achieved by playing with validator distribution
      * Then a transition block proposed by the invalid builder will have to be applied optimistically with [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) timeout -- it will give a time for importer to follow the valid chain
      * Invalid chain gets applied optimistically (because the invalid builder has more attesters and its chain is preferred by the fork choice rule) and importer's EL eventually responds with `{status: INVALID, latestValidHash: 0x00..00}` on this chain
      * CL is expected to invalidate the invalid chain blocks and switch back to the minor but valid chain
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/569)] `TTD` before Bellatrix
  <details>
  <summary>Click for details</summary>
  
  * `Genesis <- ... <- Bn`
  * EL clients start with `Bn` as head of the proof of work chain
  * `Bn` is a valid terminal block for all clients
  * Bellatrix epoch is reached after all clients have incorporated `Bn`
  * Transition is successful as CL clients build on top of `Bn` once Bellatrix is reached.
  
  </details>

## [BONUS] `TERMINAL_BLOCK_HASH`

Scenarios in this section are covering the case when a terminal PoW block is designated by specifying a certain `blockHash` value which is specified by `TERMINAL_BLOCK_HASH` parameter.

This scenario is also known as `TERMINAL_BLOCK_HASH` override and is an emergency case scenario. On EL side the override involves specifying the following parameters (as per [EIP-3675](https://eips.ethereum.org/EIPS/eip-3675#terminal-pow-block-overriding)):
* `TERMINAL_BLOCK_HASH` – set to the hash of a certain block to become the terminal PoW block.
* `TERMINAL_BLOCK_NUMBER` – set to the number of a block designated by `TERMINAL_BLOCK_HASH`.
* `TERMINAL_TOTAL_DIFFICULTY` – set to the total difficulty value of a block designated by `TERMINAL_BLOCK_HASH`.

On CL side the override has to be enabled via updating [the following parameters](https://github.com/ethereum/consensus-specs/blob/dev/specs/bellatrix/beacon-chain.md#transition-settings):
* `TERMINAL_BLOCK_HASH` – set to the hash of a certain block to become the terminal PoW block.
* `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH` - set to the epoch when the Merge transition will be activated.


### EL client tests

EL client tests for `TBH` (`TERMINAL_BLOCK_HASH`) override are design with the following *potential* implementation in mind:
* EL always triggers the Merge upgrade upon reaching `TERMINAL_TOTAL_DIFFICULTY`
* `TERMINAL_BLOCK_HASH` + `TERMINAL_BLOCK_NUMBER` whitelists the canonical chain

Effectively, this semantics gives the desired result. But if EL has two chains that have reached `TTD`, EL may not reject a transition payload built atop of a block which `blockHash != TBH`. But we have CL side to enforce the terminal block's `blockHash == TBH`. Which means that ommitting this check is OK on EL side. Tests in this section *doesn't* assume that `blockHash == TBH` is enforced by EL client implementations.

With the above thoughts in mind, the goal of this section is to check that `TBH` override doesn't affect `TTD` block condition.

* [ ] Propose valid transition block with enabled `TBH` override
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB`, `TB` is a valid terminal block, `TB.blockHash == TBH`
  * `B: Genesis <- ... <- Bn`, `B` chain is heavier than `A` and would be canonical unless `TBH` override is activated
  * EL starts with `A` and `B` imported
  * `forkchoiceUpdated(headBlockHash: TB.blockHash, payloadAttributes: mergeTransitionBlockAttributes)`
    * EL's head is set to `headBlockHash`
    * EL returns `{status: VALID, payloadId: mergeTransitionPayloadId}`
  * `getPayload(mergeTransitionPayloadId)`
    * EL returns merge transition payload
  * `newPayload(mergeTransitionPayload)`
    * EL returns `VALID`
  
  </details>

* [ ] Build atop of invalid terminal block with enabled `TBH` override
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- INV_TB`, `INV_TB` is a *valid PoW* but an *invalid terminal* block, i.e. `INV_TB.TD < TTD` and `TBH != INV_TB.blockHash`
  * EL starts with `INV_TB` as the head
  * `forkchoiceUpdated(headBlockHash: TB.blockHash, payloadAttributes: mergeTransitionBlockAttributes)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00, payloadId: null}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`
  
  </details>

* [ ] Transition on a valid chain with enabled `TBH` override
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2 <- P3`, `TB` is a valid terminal block, i.e. `TB.blockHash == TBH`
  * `B: Genesis <- ... <- Bn`, `B` chain is heavier than `A` and would be canonical unless `TBH` override is activated
  * EL starts with `A` imported up to `TB` and `B` imported up to `Bn`
  * `newPayload(A.P1)`
    * EL returns `{status: VALID, latestValidHash: payload.blockHash}`
    * EL's head points to `TB`
  * `forkchoiceUpdated(head: P1, safe: 0x00..00, finalized: 0x00..00)`
    * EL returns `{status: VALID, latestValidHash: forkchoiceState.headBlockHash}`
    * EL's head updated to `P1`
    * `eth_getBlockByNumber(safe)` returns `-39001: Unknown block` error
    * `eth_getBlockByNumber(finalized)` returns `-39001: Unknown block` error
  * `newPayload(P2) + forkchoiceUpdated(head: P2, safe: P1, finalized: 0x00..00)`
    * EL's head updated to `P2`
    * `eth_getBlockByNumber(safe)` returns `-39001: Unknown block` error
    * `eth_getBlockByNumber(finalized)` returns `-39001: Unknown block` error
  * `newPayload(P3) + forkchoiceUpdated(head: P3, safe: P2, finalized: P1)`
    * EL's head updated to `P3`
    * `eth_getBlockByNumber(safe)` returns `P2`
    * `eth_getBlockByNumber(finalized)` returns `P1`
  
  </details>

* [ ] Transition on an invalid chain with enabled `TBH` override
  <details>
  <summary>Click for details</summary>

  * `Genesis <- ... <- INV_TB <- P1`, `INV_TB` is a *valid PoW* but an *invalid terminal* block, i.e. `TBH != INV_TB.blockHash` and either of the following is true:
    * [ ] `INV_TB.TD < TTD`
    * [ ] `INV_TB.parent.TD >= TTD` -- it might require a second EL with a higher `TTD` value
  * EL starts with `INV_TB` as the head
  * `newPayload(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`
  
  </details>

* [ ] Syncing with the chain having a valid transition, with enabled `TBH` override
  <details>
  <summary>Click for details</summary>

  * `A: Genesis <- ... <- Bn <- TB <- P1`, `TB` is a valid terminal block, i.e. `TB.blockHash == TBH`
  * `B: Genesis <- ... <- Bn`, `B` chain is heavier than `A` and would be canonical unless `TBH` override is activated
  * EL starts with `B` imported
  * `newPayload(P1) + forkchoiceUpdated(P1)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * EL should pull `TB <- P1` from the network
  * poll `forkchoiceUpdated(P1)`
    * EL returns `{status: VALID, latestValidHash: null}`
    * EL's head points to `P1`
  
  </details>

* [ ] Stop processing synced PoW blocks with `TBH` override enabled
  <details>
  <summary>Click for details</summary>

  * PoW client starts with `Genesis <- ... <- TB <- B1 <- B2 <- ... <- Bn` chain, where `TB` is a valid terminal block, i.e. `TB.blockHash == TBH`
  * EL connects to the PoW client and syncs with the advertised chain
    * EL's head points to `TB`
  
  </details>

### CL client tests

In this section builder node means the one that is running all validators on it and thus builds the chain.
An importer is a node that imports the chain built by and received from the builder.

* [ ] Transition on a valid chain with `TBH` override enabled
  <details>
  <summary>Click for details</summary>

  * `EL.A: Genesis <- ... <- TB`, `TB` is a valid terminal block. i.e. `TB.blockHash == TBH`
  * `EL.B: Genesis <- ... <- Bn`, `B` chain is heavier than `A` and would be canonical unless `TBH` override is activated
  * `CL: Genesis <- ... <- Bellatrix`
  * EL starts with imported `A` and `B`
  * `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH` is greater than `BELLATRIX_FORK_EPOCH` 
  * CL starts with `Genesis` and builds a chain up to `Bellatrix` and upgrades to `Bellatrix`
    * Merger transition starts only when `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH` is reached
  * CL drives EL through transition and finalizes it
    * The terminal block is `EL.A.TB`, and the PoS chain is built atop of that block
    * `eth_getBlockByNumber(latest)` returns the head
    * `eth_getBlockByNumber(safe)` returns the most recent justified block
    * `eth_getBlockByNumber(finalized)` returns the most recent finalized block
  
  </details>

* [ ] Transition before `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH`
  <details>
  <summary>Click for details</summary>

  * `EL: Genesis <- ... <- Bn`
  * Nodes: builder, importer; importer is connected to builder on both layers
  * `Bn` is a valid terminal block to the builder's and importer's observation, i.e. `TBH == Bn.blockHash`
  * `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH` on importer's side is much grater than on builder's side
  * EL starts with `Bn` as the head
  * The builder starts with `Genesis` and build a chain up to Bellatrix and beyond
    * Importer does never imports a transition block and its head does always point to pre-transition block
  
  </details>

* [ ] Transition with mismatched `TERMINAL_BLOCK_HASH`
  <details>
  <summary>Click for details</summary>

  * `EL: Genesis <- ... <- Bn`
  * Nodes: builder, importer; importer is connected to builder on both layers
  * `Bn` is a valid terminal block to the builder's observation, i.e. builder's `TBH == Bn.blockHash`
  * `Bn` is an *invalid* terminal block to the importer's observation, i.e. builder's `TBH != Bn.blockHash`
  * `TERMINAL_BLOCK_HASH_ACTIVATION_EPOCH` is the same on buidler's and importer's sides
  * EL starts with `Bn` as the head
  * The builder starts with `Genesis` and build a chain up to Bellatrix and beyond
    * Importer does never imports a transition block and its head does always point to pre-transition block
  
  </details>
