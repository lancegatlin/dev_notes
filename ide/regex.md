

Convert "param = ???" CSL to NSL
Find: `, `
Replace: `,\n`


Convert "param = ???" NSL to "param = param"
Find: `(\s*)(\w+) = \?\?\?(,*)`
Replace: `$1$2 = $2$3`

Convert "param: Type = ???" to ""

Convert `param: Type[ = defaultValue][,]` to `param = param`
Find: `^(\s*)(\w+):.*`
Replace: `$1$2 = $2,`

Convert `param: Type[ = defaultValue][,]` to `param shouldBe defaultValue`
Find: `^(\s*)(\w+):[^=]+=\s*([^,]+),`
Replace: `$1$2 shouldBe $3`
