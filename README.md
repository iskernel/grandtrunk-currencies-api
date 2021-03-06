ServiceClients.GrandTrunkCurrencies
=========================

C# API for the http://currencies.apps.grandtrunk.net/ web service. 

The web service allows the following:
  
  - Getting a list of the supported currencies according to a specified date
  - Converting a currency to another according to the rate of a specified date
  - Retrieving a list of conversion rates between 2 currencies according to a specified time interval.

Usage
--------------

```csharp
using System;
using System.Collections.Generic;
using IsKernel.ServiceClients.GrandTrunk.HistoricCurrencyConverter.Clients.Abstract;
using IsKernel.ServiceClients.GrandTrunk.HistoricCurrencyConverter.Clients.Concrete;
using IsKernel.ServiceClients.GrandTrunk.HistoricCurrencyConverter.Contracts;

namespace IsKernel.ServiceClients.GrandTrunk.HistoricCurrencyConverter.ConsoleExample
{
	class Program
	{		
		public static void Main(string[] args)
		{
			DateTime olderDate = new DateTime(1997,3,1);
			DateTime startDate = new DateTime(2011,3,1);
			DateTime stopDate  = new DateTime(2011,5,1);
			
			IGrandTrunkCurrencyConversionService service 
				= new GrandTrunkCurrencyConversionService();
			
			//List of supported currencies at this momment
			Console.WriteLine("List of supported currencies at this moment: ");		
			List<string> listOfCurrencies
			=  service.GetListOfSupportedCurrenciesAsync().Result;
			DisplayList(listOfCurrencies);
			
			//List of supported currencies in 01-03-1997
			Console.WriteLine("List of supported currencies in 01-03-1997: ");
			List<string> olderListOfCurrencies
			= service.GetListOfSupportedCurrenciesAsync(olderDate).Result;
			DisplayList(olderListOfCurrencies);
			
			//The USD (US Dollar) - AUD (Australian Dollar) rate today
			Console.WriteLine("The USD-AUD rate today");
			ConversionRate usdToAud = service.GetConversionRateAsync("USD", "AUD").Result;
			Console.WriteLine("In {0} : 1 {1} = {3} {2}",
			                  usdToAud.Date, 
			                  usdToAud.FromCurrency,
			                  usdToAud.ToCurrency,
			                  usdToAud.Rate);
			
			//The USD (US Dollar) - AUD (Australian Dollar) rate in 01-03-1997
			Console.WriteLine("The USD-AUD rate in 01-03-1997");
			ConversionRate olderUsdToAud = service.GetConversionRateAsync("USD", "AUD", olderDate).Result;
			Console.WriteLine("In {0} : 1 {1} = {3} {2}",
			                  olderUsdToAud.Date, 
			                  olderUsdToAud.FromCurrency,
			                  olderUsdToAud.ToCurrency,
			                  olderUsdToAud.Rate);
			
			//The USD (US Dollar) - AUD (Australian Dollar) rate between 
			//01.03.2011 - 01.05.2011
			Console.WriteLine("The USD-AUD rate between 01-03-2011 and " +
			                  "01-05-2011");
			List<ConversionRate> conversionRateList
				=  service.GetConversionRateAsync("USD", "AUD", startDate, stopDate).Result;
			DisplayConversionList(conversionRateList);
			
			Console.Read();
		}
		
		private static void DisplayList(List<string> list)
		{
			foreach (string element in list) 
			{
				Console.WriteLine(element);
			}
		}
		
		private static void DisplayConversionList(List<ConversionRate> list)
		{
			foreach (ConversionRate element in list) 
			{
				Console.WriteLine("In {0} : 1 {1} = {3} {2}",
			                  	  element.Date, 
			                  	  element.FromCurrency,
			                  	  element.ToCurrency,
			                  	  element.Rate);
			}
		}
	}
}
```

Version
-

2.0

License
-

MIT
