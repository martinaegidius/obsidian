Your validation-split is NOT deterministic


fix callscript. Issue lies with OVERFIT-bool, which i think should be omitted for now.
Fix overfit-bool, as problem right now is that it tries to split trainset = 1 into valset and trainset, which of course is impossible. 