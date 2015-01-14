---
layout: post
title: 十六进制串与二进制串的转换
category: Program
tags: c++
---
支持NDK的工具函数。


```c++
#include <cstdlib>
#include <string>
#include <sstream>
#include <bitset>
#include <iomanip>

//二进制串转HEX串。如：0111001001 => 01C9
//注意：未检查参数合法性，调用时需保证参数只含0和1。
string Bin2Hex(const string &s_bin)
{
	if (s_bin.empty())
	{
		return "";
	}
	string strBin(s_bin);
	ostringstream oss;

	int rem = strBin.length() % 8;
	if (rem != 0)
	{
		strBin = string(8 - rem, '0') + strBin;
	}
    /*
	for (int i = 0, len = strBin.length(); i < len; i +=  4)
	{
		bitset<4> bsChar(strBin.substr(i, 4));
		oss << hex << uppercase << bsChar.to_ulong();
	}
    */
	for (int i = 0, len = strBin.length(); i < len; i += 8)
	{
		bitset<8> bsChar(strBin.substr(i, 8));
		oss << hex << uppercase << setw(2) << setfill('0') << bsChar.to_ulong();
	}
	return oss.str();
}
```

```c++
//将HEX串转为二进制串。如：1C9 => 000111001001
//备注：由于NDK中不支持itoa，故采用此法。
string Hex2Bin(const string &s_hex)
{
	if (s_hex.empty())
	{
		return "";
	}
	string strHex(s_hex);
	ostringstream oss;

	for (int i = 0, len = strHex.length(); i < len; ++i)
	{
		string strChar = strHex.substr(i, 1);
		long iChar = strtol(strChar.c_str(), NULL, 16);
		bitset<4> bsChar(iChar);
		oss << bsChar;
	}
	return oss.str();
}
```
