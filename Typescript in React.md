
# Typing Variables

```ts
let url: string = "https://google.com"
```

# Typing Functions

```ts
function convertCurrency(amount: number, currency: string) {
	
}

convertCurrency("blah", "US") // Error!
convertCurrency(5, "US") // Nice
```

# Typing Props

```ts
export default function Button({
	backgroundColor,
	fontSize,
	pillShape
}: {
	backgroundColor: string;
	fontSize: number;
	pillShape: boolean;
})
```

# Extracting Props

```ts
type ButtonProps {
	backgroundColor: string;
	fontSize: number;
	pillShape: boolean;
}
```

```ts
export default function Button({
	backgroundColor,
	fontSize,
	pillShape
}: {
	 ButtonProps
})
```

هنا في مشكلة بقا! ازاي؟ لو حبيت بس اعمل button استخدم ال backgroundColor prop بس فيديني error ويطلب مني باقي ال props .. الحل هو ال optional! ودا شكلها:

```ts
type ButtonProps {
	backgroundColor?: string;
	fontSize?: number;
	pillShape?: boolean;
}
```

طب انت حابب ت force بعض القيم .. يعني مثلا في ال backgroundColor حابب يكون م بين 3 خيارات فقط: red, blue or green تعملها ازاي؟

# Union Type

```ts
type ButtonProps {
	backgroundColor?: "red" | "blue" | "green";
	fontSize?: number;
	pillShape?: boolean;
}
```

طب لو عندك أكتر من prop بيستخدم نفس القيم؟ هنا يجي دور ال variable:

```ts
type Color = "red" | "blue" | "green";

type ButtonProps {
	backgroundColor?: Color;
	textColor?: Color;
	fontSize?: number;
	pillShape?: boolean;
}
```

# Typing Arrays

```ts
type ButtonProps {
	backgroundColor?: Color;
	padding: number[]
}
```

# Typing Tuples

```ts
type ButtonProps {
	backgroundColor?: Color;
	border: [number, number, number, number, string]
}

// padding: [5, 10, 5, 10, "red"]
```

# React.CSSProperties

بدل م تفضل تعمل prop لكل property في css لا هتستخدم ال properties الموجودة في css بالفعل:

```ts
type ButtonProps = {
	style: React.CSSProperties
}

export default function Button( { style }: ButtonProps ) {}
```

```tsx
<Button
 style={{
   backgroundColor: "blue",
   fontSize: 24,
   color: "white",
   ...
 }}
 />
```

# Record Type

تخيل معايا مثلا عاوز ال Button يستقبل prop إسمه borderRadius ويكون بالشكل دا:

```tsx
<Button
 borderRadius={{
   'topLeft': 5,
   'topRight': 10,
   'bottomLeft': 5,
   'bottomRight': 10,
 }}
 />
```

في ال Button.tsx هتعملها ازاي؟ ممكن تقولي اعملها بالشكل دا:

```ts
type ButtonProps = {
	borderRadius: {
	   topLeft: number;
	   topRight: number;
	   bottomLeft: number;
	   bottomRight: number;
	}
}

export default function Button({ borderRadius }: ButtonProps) {

}
```

في طريقة افضل وهي ال Record

```ts
type ButtonProps = {
	borderRadius: Record<string:number>
					  // key   : value
}
```

# Typing Prop Function 

```ts
type ButtonProps = {
	onClick: () => void | number;
}

export default function Button({ onClick }: ButtonProps) {

}
```

# Typing Children (`React.ReactNode`)

```ts
type ButtonProps = {
	children: React.ReactNode
}


export default function Button({ children }: ButtonProps) {
	return <button>{children}</button>
}
```

# `React.JSX.Element` vs `React.ReactNode`

Home.tsx:
```tsx
export default function Home() {
 const icon = <i></i>;
 return (
	 <main>
		 <Button>{icon}</Button>
	 </main>
 )
}
```

Button.tsx:
```tsx
type ButtonProps = {
	children: JSX.Element
}

export default function Button({ children }: ButtonProps) {
	return <button>{children}</button>
}
```

لو حطيت text في ال button هيطلعلك error بيقولك لازم يكون JSX Element!


# Type Alias vs Interface

في فروق كتيرة بس علشان اختصر عليك استخدم type ومتستخدمش interface.


# `ComponentProps<T>`

```tsx
type InputProps = ComponentProps<"input">

const MyInput = (props: InputProps) => {
  return <input {...props} className="my-input" />;
};
```

اي فايدتها؟ كل فكرتها بس إنها بتديك autocomplete لل component بتاعك بدل م تكتب وتخطئ.

# rest and ...spread

Home.tsx:
```tsx
export default function Home() {
	return (
		<main>
			<Button type="submit" autoFocus={true} defaultValue="test" className="btn-primary"/>
		</main>
	)
}
```


Button.tsx:
```tsx
type ButtonProps = React.ComponentPropsWithoutRef<"button">;

export default function Button({ type, autoFocus, ...rest }: ButtonProps) {
	return <button type={type} autoFocus={autoFocus} {...rest}>{children}</button>
}
```

# Intersection (&)

دي بإختصار هو جمع م بين 2 Types!

```tsx
type ButtonProps = {
	type: "button" | "submit" | "reset";
	color: "red" | "blue" | "green";
}

type SuperButtonProps = ButtonProps & {
	size: "md" | "lg";
}
```

# Interface extends

```tsx
interface ButtonProps {
	type: "button" | "submit" | "reset";
	color: "red" | "blue" | "green";
}

interface SuperButtonProps extends ButtonProps {
	size: "md" | "lg";
}
```

# Typing UseState hook

```tsx
type User = {
	name: string;
	age: number;
}

const [user, setUser] = useState<User | null>(null);

const name = user?.name;
```

# Typing useRef hook

```tsx
const ref = useRef<HTMLButtonElement | null>(null);
```

# as const

```tsx
const buttonTextOptions = [
	"Click me!",
	"CLick me again!"
] as const;
```

هتخلي ال array دي readonly يعني متقدرش تعدل عليها فقط تقرأ منها!

```ts
const arr = [1, 2, 3] as const;

arr.push(4);

// Error: Property 'push' does not exist on type 'readonly [1, 2, 3]'.
```

# Omit utility

تخيل عندك 2 types مثلا User and Guest وال Guest ليه نفس خصائص ال User بس مثلا في property موجودة في ال User مش موجودة في ال Guest!

```tsx
type User = {
	sessionID: string;
	name: string;
}

type Guest = Omit<User, "name">;
```

هنا ال Guest هياخد كل ال properties من ال User ما عدا ال user property!

# `as` Type Assertion

```tsx
type ButtonColor = "red" | "blue" | "green";

const prevButtonColor = localStorage.getItem("buttonColor") as ButtonColor;
```

بدون ال `as ButtonColor` قيمة ال prevButtonColor كانت هتكون يا  string يا null لكن دلوقتي بقت اي واحدة من الخيارات دي!

# Generics

عندك function عاوزها تقبل اكتر من نوع؟ ممكن تقولي استخدم any بالشكل التالي:

```tsx
const convertToArray = (value: any): any[] => {
	return [value];
}

convertToArray(5)
convertToArray("test")
```

مفيش مشاكل صح؟ صح .. طب لو حبيت بس اضيف toUpperCase() بالشكل التالي:

```tsx
const convertToArray = (value: any): any[] => {
	return [value.toUpperCase()];
}

convertToArray(5)
convertToArray("test")
```

مش هيقول لأ بس تيجي تشغل الكود هيديك error! لإن ال number مفيهوش function بالأسم دا .. طب والحل؟ Generics


```tsx
const convertToArray = <T,>(value: T): T[] => {
	return [value.toUpperCase()];
}

convertToArray(5)
convertToArray("test")
```

هنا بتقول النوع ترجعله نوعه وطبعا T وتقدر تسميه K .. لكن دا المتعارف عليه .. ليه بقا ال comma اللي بعد ال T في `<T,>` دي لإنك في ملف tsx والكومبايلر بدون ال comma ممكن يفكرك بتعمل component جديد! ممكن تحولها لفانكشن أفضل:

```tsx
function convertToArray<T>(value: T): T[] {
	return [value.toUpperCase()];
}

convertToArray(5)
convertToArray("test")
```

# Generics in React

Button.tsx
```tsx
type ButtonProps = {
	countValue: number;
	countHistory: number[];
}
```

لو جيت مثلا تستدعي ال Button Component دي  بالشكل دا:

```tsx
<Button countValue=5 countHistory=[10, 20, 30, 40]/>
```

لو جيت تغير نوع ال countValue ل string هيتغير عادي وتحط قيمة string عادي ودا كله ملوش علاقة بال countHistory .. طب انا عاوز الاتنين يكون م بينهم relationship في النوع الاتنين دايما يكونو نفس النوع اي الحل؟

```tsx
type ButtonProps<T> = {
	countValue: T;
	countHistory: T[];
}

export default function Button<T>({
	countValue,
	countHistory
}: ButtonProps<T>) {

}
```

كدا انت خليت الاتنين يعتمدو علي بعض ويكونو من نفس النوع!

# Importing a Type

types.ts:
```tsx
export type Color = "red" | "blue" | "green"
```

```tsx
import { type Color } from "../lib/types.ts"
```

# `unknown` Type

أفضل من any في حتة انك تستدعي API بس مش عارف اي اللي هيرجع منها! شوف دا علشان تعرف الفرق:

```ts
let value: any;

value = true; // OK
value = 42; // OK
value = "Hello World"; // OK
value = []; // OK
value = {}; // OK
value = Math.random; // OK
value = null; // OK
value = undefined; // OK
value = new TypeError(); // OK
value = Symbol("type"); // OK

value.foo.bar; // OK
value.trim(); // OK
value(); // OK
new value(); // OK
value[0][1]; // OK
```

```ts
let value: unknown;

value = true; // OK
value = 42; // OK
value = "Hello World"; // OK
value = []; // OK
value = {}; // OK
value = Math.random; // OK
value = null; // OK
value = undefined; // OK
value = new TypeError(); // OK
value = Symbol("type"); // OK

let value1: unknown = value; // OK
let value2: any = value; // OK
let value3: boolean = value; // Error
let value4: number = value; // Error
let value5: string = value; // Error
let value6: object = value; // Error
let value7: any[] = value; // Error
let value8: Function = value; // Error

```

```ts
let value: unknown;

value.foo.bar; // Error
value.trim(); // Error
value(); // Error
new value(); // Error
value[0][1]; // Error
```

```ts
const value: unknown = "Hello World";
const someString: string = value as string;
const otherString = someString.toUpperCase(); // "HELLO WORLD"
```

```ts
const value: unknown = 42;
const someString: string = value as string;
const otherString = someString.toUpperCase(); // BOOM
```

```ts
type UnionType1 = unknown | null; // unknown
type UnionType2 = unknown | undefined; // unknown
type UnionType3 = unknown | string; // unknown
type UnionType4 = unknown | number[]; // unknown
```

```ts
type UnionType5 = unknown | any; // any
```

```ts
type IntersectionType1 = unknown & null; // null
type IntersectionType2 = unknown & undefined; // undefined
type IntersectionType3 = unknown & string; // string
type IntersectionType4 = unknown & number[]; // number[]
type IntersectionType5 = unknown & any; // any
```

# ts-reset

**Without `ts-reset`**:

- `.json` (in `fetch`) and `JSON.parse` both return `any`
- `.filter(Boolean)` doesn't behave how you expect
- `array.includes` often breaks on readonly arrays

`ts-reset` smooths over these hard edges, just like a CSS reset does in the browser.

**With `ts-reset`**:

- `.json` (in `fetch`) and `JSON.parse` both return `unknown`
- `.filter(Boolean)` behaves EXACTLY how you expect
- `array.includes` is widened to be more ergonomic
- And several more changes!