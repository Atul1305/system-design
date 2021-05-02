## Stock Exchange
	This is general design of a stock-exchange which has some specifications.

# Goals
	* 100K Orders/Sec
	* 5K Stocks
	* GeoSpecific
	* Low-Latency
	* Highly Available + Scalable
	
# Requirements
	* Buy/Sell
	* Real-time price info
	* Order History
	* Risk management


Order{
	Id,
	Price,
	Quantity,
	Status,
	metadata // an object containing detail of security
}