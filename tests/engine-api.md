# Engine API tests

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [EL client tests](#el-client-tests)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## EL client tests

All test cases described in this section are beginning in a post-Merge world, i.e. `Genesis` is a terminal PoW block.

* [x] [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/enginetests.go#L610)] Invalid `block_hash`
  <details>
  <summary>+ Click for details</summary>
  
  * [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/enginetests.go#L578)] test should cover `block_hash` validation when EL is `SYNCING` and isn't `SYNCING` to be sure that sync doesn't affect the validation
  * test should cover all possible inconsistencies of `block_hash` that are fairly easy to do, i.e. random hash, hash of a block if it were a valid PoW block, etc
  * [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/enginetests.go#L610)] EL responds with `{status: INVALID_BLOCK_HASH, latestValidHash: null, validationError: errorMessage | null}`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/7d24e9bcf30dc6546fb821848ff0c8d279a80eaa/simulators/ethereum/engine/clmock.go#L244)] `VALID` *canonical* chain payload
  <details>
  <summary>+ Click for details</summary>
  
  * `P` is a `VALID` payload extending *canonical* chain
  * `newPayload(P)`
    * [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/clmock.go#L248)] EL responds with `{status: VALID, latestValidHash: payload.blockHash, validationError: null}`
    * [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/enginetests.go#L977)] EL didn't update the head (it's still set to the parent of `P`)
  * `forkchoiceUpdated(headBlock: P)`
    * [[Hive PR](https://github.com/ethereum/hive/pull/534)] EL responds with `{payloadStatus: {status: VALID, latestValidHash: forkchoiceState.headBlockHash, validationError: null}, payloadId: null}`
    * [[Hive](https://github.com/ethereum/hive/blob/ee8d44878b25fa3dec59e2536977af8a44b345dd/simulators/ethereum/engine/enginetests.go#L1002)] EL sets head to `P`
  
  </details>

* [x] [[Hive PR](https://github.com/ethereum/hive/pull/535)] Inconsistent `ForkchoiceState`
  <details>
  <summary>+ Click for details</summary>
  
  * `A: Genesis <- P1 <- P2 <- P3`, `B: Genesis <- P1' <- P2' <- P3'`
  * EL client starts with fully imported `A` and `B`
  * `forkchoiceUpdated(finalized: A.P1, safe: A.P2, head: A.P3`)
    * EL successfully re-orgs to `A.P3`, `finalized` and `safe` blocks are as expected
  * `forkchoiceUpdated(finalized: A.P1, safe: A.P2, head: B.P3'`)
  * `forkchoiceUpdated(finalized: A.P1, safe: B.P2', head: A.P3`)
  * `forkchoiceUpdated(finalized: B.P1', safe: A.P2, head: A.P3`)
    * `{error: {code: -38002, message: "Invalid forkchoice state"}}` in all cases listed above
  * `forkchoiceUpdated(finalized: B.P1', safe: B.P2', head: B.P3'`)
    * EL successfully re-orgs to `B.P3`, `finalized` and `safe` blocks are as expected
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/7d24e9bcf30dc6546fb821848ff0c8d279a80eaa/simulators/ethereum/engine/enginetests.go#L695)] `INVALID` *canonical* chain payload
  <details>
  <summary>+ Click for details</summary>
  
  * `INV_P` is an `INVALID` payload extending *canonical* chain
  * `newPayload(INV_P)`
    * `{status: INVALID, latestValidHash: P.parentHash, validationError: errorMessage | null}`
    * `INV_P` isn't available via `eth_getBlockByHash`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/7d24e9bcf30dc6546fb821848ff0c8d279a80eaa/simulators/ethereum/engine/enginetests.go#L352)] Payload with unknown parent
  <details>
  <summary>+ Click for details</summary>
  
  * `A: Genesis <- P1 <- P2 <- P3`, `B:  Genesis <- P1' <- P2' <- P3'`
  * EL client starts with `Genesis` block and state
  * `forkchoiceUpdated(A.P1)`
    * `{status: SYNCING}`
  * `newPayload(A.P1) + forkchoiceUpdated(A.P1)`
    * poll `forkchoiceUpdated(A.P1)` until it responds `{status: VALID}`, head is set to `A.P1`
  * `newPayload(B.P2')`
    * `{status: SYNCING}`
  * `newPayload(B.P1') + newPayload(B.P2') + forkchoiceUpdated(B.P2')`
    * poll `forkchoiceUpdated(B.P2')` until it responds `{status: VALID}`, head is set to `B.P2'`
  * `forkchoiceUpdated(A.P1)`
    * re-orgs back to `A.P1`
  * `newPayload(A.P3)`
    * `{status: SYNCING}`
  * `newPayload(A.P2) + newPayload(A.P3) + forkchoiceUpdated(A.P3)`
    * poll `forkchoiceUpdated(A.P3)` until it responds `{status: VALID}`, head is set to `A.P3'`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/526)] `SYNCING` with *invalid* chain
  <details>
  <summary>+ Click for details</summary>
  
  * `A: Genesis <- P1 <- P2 <- P3 <- P4`, `B: Genesis <- P1' <- INV_P2' <- P3' <- P4'`, `INV_P2'` is invalid payload
  * EL client starts with `A: P4` block and state
  * `newPayload(INV_P2') + forkchoiceUpdated(head: INV_P2')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * EL pulls `P1'` from a remote peer on the network
  * `newPayload(P3')`
    * poll `newPayload(P3')` until response is `INVALID`, with `latestValidHash: P1'.blockHash`
    * `finalized`, `safe` and head blocks didn't change, i.e. are from `A` chain
  * `newPayload(P2') + forkchoiceUpdated(head: P2')`
  * EL pulls `P1'` from a remote peer on the network
  * poll `forkchoiceUpdated(P2')` until response is `INVALID`, with `latestValidHash: P1'.blockHash`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/blob/7d24e9bcf30dc6546fb821848ff0c8d279a80eaa/simulators/ethereum/engine/clmock.go#L295)] Build a payload
  <details>
  <summary>+ Click for details</summary>
  
  * `Genesis <- P1`
  * EL clients starts with `Genesis` block and state
  * `newPayload(P1)`
    * succeedes
  * `getPayload(payloaId: random)`
    * `{error: {code: -38001, message: "Unknown payload"}}`
  * `forkchoiceUpdated(P1, payloadAttributes: {validTimestamp, validPrevRandao, validFeeRecipient})`
    * remember `existingPayloadId` returned from this call
  * `getPayload(payloaId: random)`
    * `{error: {code: -38001, message: "Unknown payload"}}`
  * `getPayload(payloaId: existingPayloadId)`
    * remember `returnedPayload` from this call
  * `newPayload(returnedPayload)`
    * `{status: VALID}`
  * `forkchoiceUpdated(returnedPayload)`
    * `{status: VALID}`, `returnedPayload` becomes the head
  * `forkchoiceUpdated(returnedPayload, payloadAttributes: {validTimestamp, validPrevRandao, validFeeRecipient})`
    * remember `existingPayloadId2` returned from this call
  * `getPayload(payloaId: existingPayloadId2)`
    * remember `returnedPayload2` from this call
  * `newPayload(returnedPayload2)`
    * `{status: VALID}`
    * `returnedPayload` remains the head
  * wait for 60 seconds
  * `getPayload(payloaId: existingPayloadId2)`
    * `{error: {code: -38001, message: "Unknown payload"}}`
  
  </details>

* [x] [[Hive](https://github.com/ethereum/hive/pull/527)] Invalid `PayloadAttributes`
  <details>
  <summary>+ Click for details</summary>
  
  * `Genesis <- P1`
  * EL clients starts with `Genesis` block and state
  * `newPayload(P1)`
    * succeedes
  * `forkchoiceUpdated(P1, payloadAttributes: {timestamp: 0, validPrevRandao, validFeeRecipient})`
    * `{error: {code: -38003, message: "Invalid payload attributes"}}`
    * head is set to `P1`
  
  </details>

* [ ] `VALID` *side* chain payload
  <details>
  <summary>+ Click for details</summary>
  
  * `P'` is a `VALID` payload extending *side* chain
  * `newPayload(P')`
    * Note: EL may respond with `ACCEPTED` or `VALID`
  * `forkchoiceUpdated(headBlock: P')`
    * EL responds with `{payloadStatus: {status: VALID, latestValidHash: forkchoiceState.headBlockHash, validationError: null}, payloadId: null}`
    * EL sets head to `P'`
  
  </details>

* [ ] Update finalized and safe block on canonical chain
  <details>
  <summary>+ Click for details</summary>
  
  * `Genesis <- P1 <- P2 <- P3 <- P4` is a subchain of valid payloads extending canonical chain
  * `newPayload(P1) + forkchoiceUpdated(finalized: Genesis, safe: Genesis, head: P1)`
  * `newPayload(P2) + forkchoiceUpdated(finalized: Genesis, safe: P1, head: P2)`
    * EL sets `safe` to `P1`, head to `P2`, `finalized == Genesis`
  * `newPayload(P3) + forkchoiceUpdated(finalized: P1, safe: P2, head: P3)`
    * EL sets `finalized` to `P1`, `safe` to `P2`, head to `P3`
  * `newPayload(P4) + forkchoiceUpdated(finalized: P2, safe: P3, head: P4)`
    * EL sets `finalized` to `P2`, `safe` to `P3`, head to `P4`
  
  </details>

* [ ] Update safe block on a *side* chain
  <details>
  <summary>+ Click for details</summary>
  
  * `A: Genesis <- P1 <- P2 <- P3` is a subchain of valid payloads extending canonical chain, `B: Genesis <- P1 <- P2' <- P3'` is a subchain of valid payloads extending side chain
  * import `A` and call `forkchoiceUpdated(finalized: P1, safe: P2, head: P3)`
    * EL sets `finalized` to `P1`, `safe` to `P2`, head to `P3`
  * import `B` by calling `newPayload(P2') + newPayload(P3')` and call `forkchoiceUpdated(finalized: P1, safe: P2', head: P3')`
    * note, this test might need `forkchoiceUpdated` poll as EL may respond with syncing
    * EL sets `finalized` to `P1`, `safe` to `P2'`, head to `P3'`
  
  </details>

* [ ] `SYNCING` with *valid* chain
  <details>
  <summary>+ Click for details</summary>
  
  * `Genesis <- P1 <- P2 <- P3 <- P4`
  * EL client starts with `Genesis` block and state
  * `newPayload(P3) + forkchoiceUpdated(head: P3)`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
  * EL client should pull `P1 <- P2` from a remote peer and finish the sync process successfully
  * `newPayload(P4) + forkchoiceUpdated(head: P4)`
    * poll `forkchoiceUpdated(finalized: P2, safe: P3, head: P4)` until response is `VALID`
    * `finalized`, `safe` and head blocks are set accordingly
* [ ] Re-org back to canonical chain while `SYNCING`
  <details>
  <summary>+ Click for details</summary>
  
  * `A: Genesis <- P1 <- P2 <- P3 <- P4`, `B: Genesis <- P1' <- P2' <- P3' <- P4'`
  * EL client is synced up to `A.P3` block, i.e. `A.P3` is the head
  * `newPayload(B.P4') + forkchoiceUpdated(head: B.P4')`
    * EL responds with `{status: SYNCING, latestValidHash: null, validationError: null}`
    * Note, the rest of `B` chain should be unavailable to keep EL unable to finish its sync process
  * `newPayload(A.P4) + forkchoiceUpdated(A.P4)`
    * poll `forkchoiceUpdated(finalized: P2, safe: P3, head: P4)` until response is `VALID`
    * `finalized`, `safe` and head blocks are set accordingly
  
  </details>

