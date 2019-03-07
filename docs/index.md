# BlockChain via Python Documentation

[Qinren](http://www.qinren.tech/xin.html).

## Introduction

* Here I want to realsize the blockchain simulation system mainly in Python environment.
* This peoject have not finished yet, the codes will be updated in the future.
* There is a demo `basic_test.py` illustrates the key principle of BlockChain, i.e. the index, hash, data, timestamp(nonce value not include yet)


## basic_test.py

    # -*- coding: utf-8 -*-
	"""
	@author: hivictor
	"""
 
	import hashlib
	import datetime

	class BatteryCoin:    
		def __init__(self,index,timestamp,data,nexthash):
			self.index = index
			self.timestamp = timestamp
			self.data = data
			self.nexthash = nexthash
			self.selfhash = self.hash_BatteryCoin()
			
		def hash_BatteryCoin(self):
			sha = hashlib.sha256()#hash加密算法,SHA是hash算法的一种，所有名字sha不能修改，还有md5算法
			datastr = str(self.index) + str(self.timestamp) + str(self.data) + str(self.nexthash)
			sha.update(datastr.encode("utf-8"))#把字符串数据变成二进制
			return sha.hexdigest()
			


	def create_first_block():#创世区块
		return BatteryCoin(0, datetime.datetime.now(), "233", "0")#最后一个0相当于创世区块的当前哈希值，可以调节

	def create_blocks(last_block):#区块链的其他快
		now_index = last_block.index + 1
		now_timestamp = datetime.datetime.now()
		now_data = "233" + str(now_index)#模拟本区块的数据，可以调节
		now_hash = last_block.selfhash#本区块的哈希值就等于上一区块的哈希值
		return BatteryCoin(now_index, now_timestamp, now_data, now_hash)
		
	####################模拟创建一个区块链，链表的样子#####################
	Battery = [create_first_block()]
	blocks_num = 10
	head_block = Battery[0]#创世区块
	print(head_block.index, head_block.timestamp, head_block.data, head_block.nexthash, head_block.selfhash)

	for block_index in range(blocks_num):
		block_add = create_blocks(head_block)
		Battery.append(block_add)
		head_block = block_add#循环，我感觉最重要的是把区块的哈希值传给了下一个区块
		print(block_add.index, block_add.timestamp, block_add.data, block_add.selfhash, block_add.nexthash)
