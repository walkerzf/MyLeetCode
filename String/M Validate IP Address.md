## Validate IP Address

> Write a function to check whether an input string is a valid IPv4 address or IPv6 address or neither.
>
> **IPv4** addresses are canonically represented in dot-decimal notation, which consists of four decimal numbers, each ranging from 0 to 255, separated by dots ("."), e.g.,`172.16.254.1`;
>
> Besides, leading zeros in the IPv4 is invalid. For example, the address `172.16.254.01` is invalid.
>
> **IPv6** addresses are represented as eight groups of four hexadecimal digits, each group representing 16 bits. The groups are separated by colons (":"). For example, the address `2001:0db8:85a3:0000:0000:8a2e:0370:7334` is a valid one. Also, we could omit some leading zeros among four hexadecimal digits and some low-case characters in the address to upper-case ones, so `2001:db8:85a3:0:0:8A2E:0370:7334` is also a valid IPv6 address(Omit leading zeros and using upper cases).
>
> However, we don't replace a consecutive group of zero value with a single empty group using two consecutive colons (::) to pursue simplicity. For example, `2001:0db8:85a3::8A2E:0370:7334` is an invalid IPv6 address.
>
> Besides, extra leading zeros in the IPv6 is also invalid. For example, the address `02001:0db8:85a3:0000:0000:8a2e:0370:7334` is invalid.
>
> **Note:** You may assume there is no extra space or special characters in the input string.
>
> **Example 1:**
>
> ```
> Input: "172.16.254.1"
> 
> Output: "IPv4"
> 
> Explanation: This is a valid IPv4 address, return "IPv4".
> ```
>
> 
>
> **Example 2:**
>
> ```
> Input: "2001:0db8:85a3:0:0:8A2E:0370:7334"
> 
> Output: "IPv6"
> 
> Explanation: This is a valid IPv6 address, return "IPv6".
> ```
>
> 
>
> **Example 3:**
>
> ```
> Input: "256.256.256.256"
> 
> Output: "Neither"
> 
> Explanation: This is neither a IPv4 address nor a IPv6 address.
> ```



## Solution  respectively test

* first test the number of  dot or :     3 or 7 
* then respectively test every String 
  * split with  . or :  ```\\.```  ```\\:```
  * corner case 
    * for IPV4  
      * every substring length
      * no leading zero but only one zero 
      * every bit is digit 
      * the while number  0~255
    * for IPV6
      * substring length
      * can have leading zero 
      * every bit is valid hexadecimal

```java
class Solution {
  public String validateIPv4(String IP) {
    String[] nums = IP.split("\\.", -1);
    for (String x : nums) {
      // Validate integer in range (0, 255):
      // 1. length of chunk is between 1 and 3
      if (x.length() == 0 || x.length() > 3) return "Neither";
      // 2. no extra leading zeros
      if (x.charAt(0) == '0' && x.length() != 1) return "Neither";
      // 3. only digits are allowed
      for (char ch : x.toCharArray()) {
        if (! Character.isDigit(ch)) return "Neither";
      }
      // 4. less than 255
      if (Integer.parseInt(x) > 255) return "Neither";
    }
    return "IPv4";
  }

  public String validateIPv6(String IP) {
    String[] nums = IP.split(":", -1);
    String hexdigits = "0123456789abcdefABCDEF";
    for (String x : nums) {
      // Validate hexadecimal in range (0, 2**16):
      // 1. at least one and not more than 4 hexdigits in one chunk
      if (x.length() == 0 || x.length() > 4) return "Neither";
      // 2. only hexdigits are allowed: 0-9, a-f, A-F
      for (Character ch : x.toCharArray()) {
        if (hexdigits.indexOf(ch) == -1) return "Neither";
      }
    }
    return "IPv6";
  }

  public String validIPAddress(String IP) {
    if (IP.chars().filter(ch -> ch == '.').count() == 3) {
      return validateIPv4(IP);
    }
    else if (IP.chars().filter(ch -> ch == ':').count() == 7) {
      return validateIPv6(IP);
    }
    else return "Neither";
  }
}


```

```java
class Solution {
    public String validIPAddress(String IP) {
         if(IP.chars().filter(ch ->ch=='.').count()==3) 
             return validateIpv4(IP); 
        else if(IP.chars().filter(ch ->ch==':').count()==7)
             return validateIpv6(IP);
        return "Neither";
    }
    private String  validateIpv4(String IP){
        String s [] =IP.split("\\.",-1);
        for(String a :s){
            if(a.length()==0||a.length()>3) return "Neither";
            if(a.length()!=1&&a.charAt(0)=='0') return "Neither";
            for(int i=0;i<a.length();i++){
                if(!Character.isDigit(a.charAt(i))) return "Neither";
            }
            if(Integer.parseInt(a)>255) return "Neither"; 
        }
        return "IPv4";
    }
    private  String validateIpv6(String IP){
        Set <Character> s =new HashSet<>();
        s.add('0');s.add('1');
        s.add('2');s.add('3');
        s.add('4');s.add('5');
        s.add('6');s.add('7');
        s.add('8');s.add('9');
        s.add('A');s.add('B');
        s.add('C');s.add('D');
        s.add('E');s.add('F');
        s.add('a');s.add('b');
        s.add('c');s.add('d');
        s.add('e');s.add('f');
        String ss[] =IP.split("\\:",-1);
        for(String a:ss){
            if(a.length()==0||a.length()>4) return "Neither";
            for(int i=0;i<a.length();i++){
                if(!s.contains(a.charAt(i))){
                    return "Neither";
                }
            }
        }
        return "IPv6";
    }
}
```



## Solution  Py

```python
def validIPAddress(self, IP):
        def isIPv4(s):
            try: return str(int(s)) == s and 0 <= int(s) <= 255
            except: return False
        def isIPv6(s):
            if len(s) > 4: return False
            try: return int(s, 16) >= 0 and s[0] != '-'
            except: return False
        if IP.count(".") == 3 and all(isIPv4(i) for i in IP.split(".")): 
            return "IPv4"
        if IP.count(":") == 7 and all(isIPv6(i) for i in IP.split(":")): 
            return "IPv6"
        return "Neither"
```

```java
  public static String validIPAddress(String IP) {
        String[] ipv4 = IP.split("\\.",-1);
        String[] ipv6 = IP.split("\\:",-1);
        if(IP.chars().filter(ch -> ch == '.').count() == 3){
            for(String s : ipv4) if(isIPv4(s)) continue;else return "Neither"; return "IPv4";
        }
        if(IP.chars().filter(ch -> ch == ':').count() == 7){
            for(String s : ipv6) if(isIPv6(s)) continue;else return "Neither";return "IPv6";
        }
        return "Neither";
    }
		//String.valueOf an Integer.valueOf is to judge  leading zero
		//>=0 <=255 is or not valid
		//when have special Character wll NumberFormatException
    public static boolean isIPv4 (String s){
         try{ return String.valueOf(Integer.valueOf(s)).equals(s) && Integer.parseInt(s) >= 0 && Integer.parseInt(s) <= 255;}
         catch (NumberFormatException e){return false;}
    }
		//Integer.parseInt change it to heexadecimal
		//also NumberFormatException 
    public static boolean isIPv6 (String s){
        if (s.length() > 4) return false;
        try {return Integer.parseInt(s, 16) >= 0  && s.charAt(0) != '-';}
        catch (NumberFormatException e){return false;}
    }
```

