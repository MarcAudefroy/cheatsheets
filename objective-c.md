# Objective-C

## Informations 

* Langage used by Apple
* Superset of C
* Inspired by Smalltalk (messaging system)

## Comments and pragma
```
//Comment inline

/*
Mutiple
lines
*/


#pragma mark - Name of pragma

```

## Types


### C Primitif types  


```
float myFloat = 2.0;
bool myBool = true;
int myInt = 42;
char myChar = 'c';
char myString[] = "String";
```
### Obj-C types

```
id anObject = @"MyIdobject"; // id type represents an obj-c object's pointer
NSString *myString = @"MyString";
NSNumber *MyNumber = @27; // @27.3
BOOL myBOOL = YES; // NO
Class aClass = [NSString class]; 
SEL aSelector = @selector(someMethodWithAnArg:)
Method aMethod = class_getInstanceMethod(aClass,aSelector)
IMP implementation = [anObject methodForSelector:aSelector];
```

### Enumeration et Bitmask

```
//Enumeration
typedef NS_ENUM(NSInteger, WifiState) {
    WifiStateUnknown = 1,
    WifiStateOn, // 2
    WifiStateOff, // 3
};

//Bitmask
typedef NS_OPTIONS(NSUInteger, BitMaskExample){
	BitMaskExampleFoo    = 0,		//0000
	BitMaskExampleBar    = 1 << 0,	//0001
	BitMaskExampleBeez   = 1 << 1,	//0010
	BitMaskExampleTruc   = 1 << 2,	//0100
	BitMaskExampleMachin = 1 << 3 	//1000
};
 
```

### Array

```
NSArray myArray = @[@"foo", @12, anObject];
NSString *foo = myArray[0];
myArray[0] = @"bar";

NSMutableArray mutableArray = [NSMutableArray arrayWithArray:myArray];  
[mutableArray addObject:anOtherObject]; 
[mutableArray removeObject:anOtherObject];

//Enumerate an NSArray
[myArray enumerateObjectsUsingBlock: ^(id obj, NSUInteger idx, BOOL *stop) {
    
}];
 
```

### Dictionary

```
NSDictionary myDictionary = @{@"key1":@12 , @"key2":@"foo", @"key3":anObject};
NSString *foo = myDictionary[@"key2"];
myDictionary[@"key2"] = @"bar";

NSMutableDictionary mutableDictionary = [NSMutableDictionary dictionaryWithDictionary:myDictionary];  //mutable

mutableDictionary[@"anRandomKey"] = fooObject;
[mutableArray removeObjectForKey:@"anRandomKey"];

//Enumerate an NSDictionary
[myDictionary enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {
    
}];
 
```

### Blocs

```
__block NSString *anString = @"foo";

void (^simpleBlock)(void) = ^{

	anString = @"bar"

}
    
simpleBlock(); 

double (^multiplyTwoValues)(double, double) =                             ^(double firstValue, double secondValue) {
	return firstValue * secondValue;
};

double result = multiplyTwoValues(2,4);
```

## Basics

### Conditions

```
int myInt = 1

if ( myInt == 2) 
{
	//Do Something
}
else if (myInt <= 3 && myInt > 0)
{
	//Do Something
}
else if (myInt < 0 || myInt > -3  )
{
	//Do Something
}
else if (myInt % 2 == 0)
{
	//Do Something
}
else
{
	//Do Something
}

NSObject *anObject = [NSObject new];
if([anObject isEqual:anAnotherObject])
{
	//Do Something
}

NSString *myString = @"kk"
if([myString isEqualToString:@"kk"])
{
	//Do Something
}

```  

### Loop

```
for (int i = 0; i<10; i++)
{
	//Do Something
}

int y = 0;
while (y < 3)
{
	y++;
}

do
{
	//Do Something
} while (aCondition)

NSArray *stringArray = @[@"String1", @"String2"];
for (NSString* currentString in stringArray)
{
   //Do Something with currentString
}

```

## Oriented object

### Protocols

```
//Protocol1.h

@protocol Protocol1 : NSObject

@required
-(void)requiredMethodOfProtocol1;

@optional
-(void)optionalMethodOfProtocol1:(NSString *)name;

@end

```

### Class Header 

```
//MyCustomClass.h

#import Protocol1.h
#import Protocol2.h

@interface MyCustomClass : SuperClass <Protocol1,Protocol2>


@property (nonatomic, copy) NSString 	 *name
@property (nonatomic, strong) NSNumber  *aNumber
@property (nonatomic) 		   int    	  count;

//initializer
-(instancetype)initWithName:(NSString *)name;

-(void)anInstanceMethod:(NSString*)aString withAnotherArg:(id)anObject;

+(id)aClassMethodWithoutArgs;

@end

```

#### attributes of property

attribute 	 | description 
------------ | ------------- 
strong | keep reference of object  
weak | doesn't keep reference of object
assign | no reference (default)  
copy | create a copy of object when set
readwrite | create getter and setter (default)  
readonly | create only getter  

### Class Implementation

```
//MyCustomClass.m

@interface MyCustomClass () <Protocol3> //An Extension

@property (nonatomic, strong) NSNumber *aPrivateProperty

@end

@implementation MyCustomClass

-(instancetype)initWithName:(NSString *)name
{
	self = [super init];
	if (self)
	{
		self.name = @"foo";
	}
	return self;
}

-(void)anInstanceMethod:(NSString*)aString withAnotherArg:(id)anObject
{
	//Do Something
}

+(id)aClassMethodWithoutArgs
{
	NSObject *anObject [[NSObject alloc] init];
	return anObject;
}

#pragma mark - methods Protocols

-(void)requiredMethodOfProtocol1
{
	//implementation
}

-(void)optionalMethodOfProtocol2
{
	//implementation
}
@end

```

###Category

```
//MyCustomClass+MA.h

@interface MyCustomClass (MA)

-(void)anCategoryMethod

@end
```

```
//MyCustomClass+MA.m

@implementation MyCustomClass (MA)

-(void)anCategoryMethod {
	//Implementation
}

@end
```



## Some Tips

### Create Shared Instance

```
+ (SharedInstanceClass)sharedInstance {
    static dispatch_once_t onceToken;
    static SharedInstanceClass *sharedInstance = nil;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[SharedInstanceClass alloc] init];
    });
    return sharedInstance;
}

```

### Add property to a category
```
//AcustomClass+MA.h
@interface ACustomClass (MA)

@property (nonatomic, strong) id customProperty

@end

//AcustomClass+MA.h

@implementation ACustomClass (MA)
@dynamic customProperty

-(void)setCustomProperty:(id)object 
{    
	objc_setAssociatedObject(self, @selector(customProperty), object, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

-(id)customProperty
{    
	objc_getAssociatedObject(self, @selector(customProperty));
}

@end

```

### Create asynchronous method

```
-(void)methodWithCompletion:(dispatch_block_t)completion
{
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0), ^{
    	//do something here
    	
        dispatch_async(dispatch_get_main_queue(), ^{ 
            if(completion)
            {
            	completion();
            }
        });
    });
}
```