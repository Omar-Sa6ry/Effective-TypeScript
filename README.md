TypeScript is structural
Type compatibility is based on structure, not names. If two types have the same shape, they’re considered compatible—even if they have different names.

Types only exist at compile time
TypeScript types are erased at runtime. You can’t check custom types with typeof or instanceof—those only work with real JS types like number, string, etc.

Trust the compiler, but verify
TypeScript’s inference is strong, but it can be too broad or too narrow. Be explicit when necessary, especially in public-facing APIs.

Use type inference, but be deliberate
Let the compiler infer types when it makes sense, but step in and annotate when clarity or precision is important.

Enable --strict mode and --noImplicitAny
These compiler options make your code safer by enforcing better type discipline.

Prefer unknown over any
any disables type safety. unknown requires you to check the type before using it, which leads to safer code.

Use const and readonly
Immutability helps TypeScript give you better type inference and reduces bugs. Favor const over let and use readonly in your types where possible.
