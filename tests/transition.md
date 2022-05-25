# Merge transition tests

All test cases described in this document are beginning in a pre-Merge world and reach the terminal PoW during execution to perform the PoS transition.

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [`TERMINAL_TOTAL_DIFFICULTY`](#terminal_total_difficulty)
  - [EL client tests](#el-client-tests)
  - [CL client tests](#cl-client-tests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## `TERMINAL_TOTAL_DIFFICULTY`

### EL client tests

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

* [x] [[Hive](https://github.com/ethereum/hive/blob/32ff59c89c879972f00abe62137a566def81a080/simulators/ethereum/engine/clmock.go#L346)] Propose valid transition block
  <details>
  <summary>Click for details &#9662;</summary>

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
  <summary>Click for details &#9662;</summary>

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

* [ ] Build atop of invalid terminal block
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- INV_TB`, `INV_TB` is a *valid PoW* but an *invalid terminal* block, i.e. `INV_TB.TD < TTD`; `INV_TB.parent.TD >= TTD` can't be checked easily as EL wouldn't process such block
  * EL starts with `INV_TB` as the head
  * `forkchoiceUpdated(headBlockHash: TB.blockHash, payloadAttributes: mergeTransitionBlockAttributes)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00, payloadId: null}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`
  
  </details>

* [ ] Transition on a valid chain
  <details>
  <summary>Click for details &#9662;</summary>

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

* [ ] Transition on an invalid chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- INV_TB <- P1`, `INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [ ] `INV_TB.TD < TTD`
    * [ ] `INV_TB.parent.TD >= TTD` -- it might require a second EL with a higher `TTD` value
  * EL starts with `INV_TB` as the head
  * `newPayload(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00..00}`
    * EL's head points to `INV_TB` if `INV_TB.TD < TTD`, and to `INV_TB.parent` if `INV_TB.parent.TD >= TTD`

* [ ] Re-org to chain with invalid transition block
  <details>
  <summary>Click for details &#9662;</summary>

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
    * EL's head points to `B.P1`
  
  </details>

* [ ] Syncing with the chain having a valid transition
  <details>
  <summary>Click for details &#9662;</summary>

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

* [ ] Syncing with the chain having invalid *terminal* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- Bn <- INV_TB <- P1 <- P2`, `INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [ ] `INV_TB.TD < TTD`
    * [ ] `INV_TB.parent.TD >= TTD` -- it might require a second EL with a higher `TTD` value
  * EL starts with `Bn` as the head
  * `newPayload(P1) + forkchoiceUpdated(P1)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * EL should pull `INV_TB <- P1` from the network
  * poll `forkchoiceUpdated(P1)`
    * EL returns `{status: INVALID, latestValidHash: 0x00.00}`
    * EL's head points to `Bn`
  
  </details>

* [ ] Syncing with the chain having invalid *transition* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- Bn <- TB <- INV_P1 <- P2`, `TB` is a valid terminal block, `INV_P1` is an invalid payload
  * EL starts with `Bn` as the head
  * `newPayload(P2) + forkchoiceUpdated(P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * poll `forkchoiceUpdated(P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00.00}`
    * EL's head points to `TB`
  
  </details>

* [ ] Syncing with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- Bn <- TB <- P1 <- INV_P2 <- P3`, `TB` is a valid terminal block, `INV_P2` is an invalid payload
  * EL starts with `Bn` as the head
  * `newPayload(P3) + forkchoiceUpdated(P3)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `Bn`
  * poll `forkchoiceUpdated(P3)`
    * EL returns `{status: INVALID, latestValidHash: P1.blockHash}`
    * EL's head points to `TB` (in this case `fcU` won't be applied at all because it does point to a block on an invalid chain, thus, no update and PoS transition should happen; normally, a CL will send `fcU(P1)` when get `lvh = P1.blockHash`)
  
  </details>

* [ ] Re-org and sync with the chain having invalid *terminal* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2`, `B: Genesis <- ...  <- INV_TB <- P1 <- P2`, `B.INV_TB` is a *valid PoW* but an *invalid terminal* block:
    * [ ] `INV_TB.TD < TTD`
    * [ ] `INV_TB.parent.TD >= TTD`
  * This scenario may require second EL with a higher `TTD` value
  * EL starts with `A.P2` as the head
  * `newPayload(B.P2) + forkchoiceUpdated(P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P2`
  * EL should pull `B: INV_TB <- P1 <- P2` from the network
  * poll `forkchoiceUpdated(B.P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00.00}`
    * EL's head points to `A.P2`
  
  </details>

* [ ] Re-org and sync with the chain having invalid *transition* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2`, `B: Genesis <- ... <- TB <- INV_P1 <- P2`, `B.TB` is a valid terminal block, `B.INV_P1` is an invalid payload
  * EL starts with `A.P2` as the head
  * `newPayload(B.P2) + forkchoiceUpdated(B.P2)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P2`
  * poll `forkchoiceUpdated(B.P2)`
    * EL returns `{status: INVALID, latestValidHash: 0x00.00}`
    * EL's head points to `A.P2`
  
  </details>

* [ ] Re-org and sync with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `A: Genesis <- ... <- TB <- P1 <- P2 <- P3`, `B: Genesis <- ... <- TB <- P1 <- INV_P2 <- P3`, `B.TB` is a valid terminal block, `B.INV_P2` is an invalid payload
  * EL starts with `A.P2` as the head
  * `newPayload(B.P3) + forkchoiceUpdated(B.P3)`
    * EL returns `{status: SYNCING}`
    * EL's head points to `A.P3`
  * poll `forkchoiceUpdated(B.P3)`
    * EL returns `{status: INVALID, latestValidHash: B.P1.blockHash}`
    * EL's head points to `A.P3` (in this case `fcU` won't be applied at all because it does point to a block on an invalid chain, thus, no update of the forkchoice state should happen; normally, a CL will send `fcU(B.P1)` when get `lvh = B.P1.blockHash` and switch canonical chain to `B` with `B.P1` as the head)
  
  </details>

* [ ] Stop processing gossiped PoW blocks
  <details>
  <summary>Click for details &#9662;</summary>

  * PoW/Clique miner builds a chain and advertises it via gossip; the chain goes beyond a terminal PoW block
  * EL is connected to the miner and does not process blocks after the terminal one, i.e. the head points to the terminal block
  
  </details>

* [ ] Stop processing synced PoW blocks
  <details>
  <summary>Click for details &#9662;</summary>

  * PoW client starts with `Genesis <- ... <- TB <- B1 <- B2 <- ... <- Bn` chain, where `TB` is a valid terminal block
  * EL connects to the PoW client and syncs with the advertised chain
    * EL's head points to `TB`
  
  </details>

* [ ] Terminal blocks are gossiped
  <details>
  <summary>Click for details &#9662;</summary>

  * Consider `PoW <-> EL1 <-> EL2` as a PoW client connected to EL client `EL1`, which in its turn connected to EL client `EL2`. There is no direct connection between `EL2` and `PoW` clients.
  * `PoW` client gossips a terminal block `TB`
    * Head of `EL2` client points to `TB`
  * `PoW` client gossips a descendant of `TB`
    * Head of `EL2` client points to `TB`
  
  </details>

### CL client tests

* [ ] Transition on a valid chain
  <details>
  <summary>Click for details &#9662;</summary>

  * `EL: Genesis <- ... <- TB`, `TB` is a valid terminal block
  * `CL: Genesis <- ... <- Bellatrix`
  * EL starts with `TB` as the head or mines a chain up to `TB` 
  * CL strats with `Genesis` and builds a chain up to `Bellatrix` and upgrades to `Bellatrix`
  * CL drives EL through transition and finalizes it
    * `eth_getBlockByNumber(latest)` returns the head
    * `eth_getBlockByNumber(safe)` returns the most recent justified block
    * `eth_getBlockByNumber(finalized)` returns the most recent finalized block
  
  </details>

* [ ] Build atop of invalid terminal block
  <details>
  <summary>Click for details &#9662;</summary>

  * `Genesis <- ... <- Bn`, `Bn` is a *valid PoW* but an *invalid terminal* block, i.e. `Bn.TD < EL.TTD`
  * `Bn` is a valid terminal block to CL's observation, i.e. CL's `TERMINAL_TOTAL_DIFFICULTY` is configured in the following way:
    * `Bn.TD >= CL.TTD && Bn.parent.TD < CL.TTD`
  * EL starts with `Bn` as the head
  * CL starts with `Genesis` and build a chain up to Bellatrix
    * Transition never happens, i.e. CL's head block always contains zeroed payload (`ExecutionPayload()`)
  
  </details>

* [ ] Transition on a chain with invalid *terminal* block
  <details>
  <summary>Click for details &#9662;</summary>

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

* [ ] Transition on a chain with invalid *transition* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `EL: Genesis <- ... <- Bn`
  * Nodes: builder, importer; importer is connected to builder on both layers
  * `Bn` is a valid terminal block to builder and importer observation
  * EL starts with `Bn` as the head
  * CL starts with `Genesis` and the builder builds a chain up to Bellatrix and beyond
  * EL mock of the importer node returns `INVALID` to the transition block's payload
    * Importer does never imports a transition block and its head does always point to pre-transition block
  
  </details>

* [ ] Syncing with the chain having a valid transition block
  <details>
  <summary>Click for details &#9662;</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: builder, importer; builder starts synced with `EL.A`, importer with `EL.B`, same `TD` value on both chains prevents importer from pulling builder's chain beforehand
  * The builder starts with `CL: Genesis` and builds a chain up to Bellatrix and beyond
    * Importer will have to pull `EL.A` from builder
    * Importer block processing is paused and waits for [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) to proceed -- this might worth checking
    * After the timeout exceedes importer eventually syncs and passes through the transition
  
  </details>

* [ ] Syncing with the chain having invalid *transition* block
  <details>
  <summary>Click for details &#9662;</summary>

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

* [ ] Syncing with the chain having invalid *post-transition* block
  <details>
  <summary>Click for details &#9662;</summary>

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

* [ ] Re-org and sync with the chain having invalid *terminal* block
  <details>
  <summary>Click for details &#9662;</summary>

  * `EL.A: Genesis <- ... <- Bn`, `EL.B: Genesis <- ... <- Bn`; `A` and `B` has equal `TD`
  * Nodes: valid builder, invalid builder, importer; valid builder and importer start synced with `EL.A`, invalid builder with `EL.B`, same `TD` value on both chains prevents importer and valid builder from pulling invalid builder's chain beforehand. Valid builder has `1/2n - x`, invalid builder has `1/2 + x` validators, where `x` is a small number
  * `A.Bn` is a valid terminal block to valid builder's and importer's observation
  * `B.Bn` is an *invalid* terminal block to the importer's observation, but valid to the invalid builder's observation
  * Two builders start with `CL: Genesis` and builds a chain up to Bellatrix and beyond. They should stay in consensus up to Bellatrix and then fall apart starting with a transition block
    * Importer's head eventually is coherent with a valid builder
    * Ideally, the valid builder proposes a transition block *before* the invalid builder's does the same -- these can be achived by playing with validator distribution
      * Then a transition block proposed by the invalid builder will have to be applied optimistically with [`SAFE_SLOTS_TO_IMPORT_OPTIMISTICALLY`](https://github.com/ethereum/consensus-specs/blob/dev/sync/optimistic.md#constants) timeout -- it will give a time for importer to follow the valid chain
      * Invalid chain gets applied optimistically (because the invalid builder has more attesters and its chain is preferred by the fork choice rule) and importer's EL eventually responds with `{status: INVALID, latestValidHash: 0x00.00}` on this chain
      * CL is expected to invalidate the invalid chain blocks and switch back to the minor but valid chain
  
  </details>
