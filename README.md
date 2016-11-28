# two-phase-commit
#概要
	MongoDB数据库中操作single-document总是原子性的，然而，涉及multi-document的操作，而不是原子性的。在这些情况下，使用two-phase-
	
	commit，提供这些类型的multi-document操作支持
#背景
	当执行一个由连续操作组成的事务时，某些问题出现了，比如：
	如果一个操作失败，比如，丢失信息，丢失回复，中途断电等，事务内的之前的操作必须 ” 回滚 “到之前的状态（就是 “all or nothing” 里面的 “nothing”）。
	遇到这种情况，two-pase-commit 则可以解决这些问题。
	
#执行阶段
	阶段1：请求阶段（commit-request phase，或称表决阶段，voting phase）

	在请求阶段，协调者将通知事务参与者准备提交或取消事务，然后进入表决过程。在表决过程中，参与者将告知协调者自己的决策：同意（事务参与者本地作业执	行成功）或取消（本地作业执行故障）。

	阶段2：提交阶段（commit phase）

	在该阶段，协调者将基于第一个阶段的投票结果进行决策：提交或取消。当且仅当所有的参与者同意提交事务协调者才通知所有的参与者提交事务，否则协调者将通知所有的参与者取消事务。参与者在接收到协调者发来的消息后将执行响应的操作。
	
#案例
  	假设一个情景，你想从账户 A 转钱到账户 B ，MongoDB里，你可以模仿两阶段提交来达到从 A 账户上减去钱并且为 B 账户添加上钱
	
	详情参考：http://docs.mongoing.com/manual-zh/tutorial/perform-two-phase-commits.html#pattern

  
