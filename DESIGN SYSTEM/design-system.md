## 🎨 Design System — MUST FOLLOW

### ✅ Custom `Button` Component (Replace all raw `<button>` usage)

Use this `Button` component for all button elements:

```jsx
// components/Button.tsx
import React from "react";
import classNames from "classnames";

export function Button({
  children,
  color = "blue",
  size = "md",
  variant = "solid",
  disabled = false,
  isLoading = false,
  fullWidth = false,
  iconOnly = false,
  icon: Icon,
  ...props
}) {
  const baseStyles =
    "inline-flex items-center justify-center font-semibold rounded-md transition-colors focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:pointer-events-none";

  const colorStyles = {
    blue: "bg-blue-600 text-white hover:bg-blue-700", // use this for "Ultimate" button variant
    black: "bg-black text-white hover:bg-gray-800", // use this for "Primary" button variant
    white: "bg-white text-black border border-gray-300 hover:bg-gray-100", // use this for "Secondary" button variant
    red: "bg-red-600 text-white hover:bg-red-700", // use this for "Destructive" button variant
  };

  const sizeStyles = {
    sm: "h-8 px-3 text-sm",
    md: "h-10 px-4 text-base",
    lg: "h-12 px-6 text-lg",
  };

  const variantStyles = {
    solid: "", // use this for other than "Quiet" button variants 
    ghost: "bg-transparent hover:bg-gray-100", // use this for "Quiet" button variant
  };

  const iconOnlyStyles = iconOnly ? "px-2" : "";
  const fullWidthStyles = fullWidth ? "w-full" : "w-auto";
  const loadingStyles = isLoading ? "relative pointer-events-none" : "";

  const classes = classNames(
    baseStyles,
    colorStyles[color],
    sizeStyles[size],
    variantStyles[variant],
    iconOnlyStyles,
    fullWidthStyles,
    loadingStyles
  );

  return (
    <button className={classes} disabled={disabled || isLoading} {...props}>
      {isLoading && (
        <svg
          className="animate-spin h-4 w-4 mr-2 text-white"
          xmlns="http://www.w3.org/2000/svg"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle
            className="opacity-25"
            cx="12"
            cy="12"
            r="10"
            stroke="currentColor"
            strokeWidth="4"
          />
          <path
            className="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8v4a4 4 0 00-4 4H4z"
          />
        </svg>
      )}
      {Icon && <Icon className={classNames("w-4 h-4", children && "mr-2")} />}
      {!iconOnly && children}
    </button>
  );
}
```

Usage:
```jsx
import { Button } from "./components/Button";
import { PlusIcon } from "@heroicons/react/24/solid";

export default function App() {
  return (
    <div className="space-x-4 p-4">
      <Button>Default</Button>
      <Button color="red">Destructive</Button>
      <Button size="sm">Small</Button>
      <Button size="lg" variant="ghost">Ghost</Button>
      <Button isLoading>Loading</Button>
      <Button icon={PlusIcon}>Add</Button>
      <Button icon={PlusIcon} iconOnly />
    </div>
  );
}
```
---

### ✅ Custom `Input` Component (Replace all `<input type="text">`, `email`, or `password`)

Use this `Input` component for all text-based inputs:

```jsx
// components/Input.tsx
import React from "react";
import classNames from "classnames";

export function Input({
  type = "text",
  value,
  onChange,
  placeholder = "",
  size = "md", // 'sm' | 'md'
  disabled = false,
  isInvalid = false,
  helperText = "",
  errorMessage = "",
  showCounter = false,
  maxLength,
  leftAddon,
  rightAddon,
  icon,
  iconRight,
  className = "",
  ...props
}) {
  const inputBase =
    "block w-full rounded-md border bg-white placeholder-gray-400 shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition";

  const sizeStyles = {
    sm: "text-sm px-2 py-1",
    md: "text-base px-3 py-2",
  };

  const stateStyles = classNames({
    "border-gray-300": !isInvalid && !disabled,
    "border-red-500 text-red-600 focus:ring-red-500 focus:border-red-500": isInvalid,
    "bg-gray-100 text-gray-500 border-gray-200 cursor-not-allowed": disabled,
  });

  const inputClass = classNames(
    inputBase,
    sizeStyles[size],
    stateStyles,
    className
  );

  const characterCount = showCounter && maxLength ? `${value?.length || 0}/${maxLength}` : null;

  return (
    <div className="w-full space-y-1">
      <div className="relative flex rounded-md shadow-sm">
        {leftAddon && (
          <span className="inline-flex items-center px-3 rounded-l-md border border-r-0 border-gray-300 bg-gray-50 text-gray-500 text-sm">
            {leftAddon}
          </span>
        )}
        <div className="relative flex items-center w-full">
          {icon && (
            <span className="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none text-gray-400">
              {icon}
            </span>
          )}
          <input
            type={type}
            value={value}
            onChange={onChange}
            placeholder={placeholder}
            className={classNames(
              inputClass,
              icon && "pl-10",
              iconRight && "pr-10",
              leftAddon && "rounded-l-none",
              rightAddon && "rounded-r-none"
            )}
            disabled={disabled}
            maxLength={maxLength}
            {...props}
          />
          {iconRight && (
            <span className="absolute inset-y-0 right-0 pr-3 flex items-center pointer-events-none text-gray-400">
              {iconRight}
            </span>
          )}
        </div>
        {rightAddon && (
          <span className="inline-flex items-center px-3 rounded-r-md border border-l-0 border-gray-300 bg-gray-50 text-gray-500 text-sm">
            {rightAddon}
          </span>
        )}
      </div>

      {isInvalid && errorMessage && (
        <p className="text-sm text-red-600">{errorMessage}</p>
      )}

      {!isInvalid && helperText && (
        <p className="text-sm text-gray-500">{helperText}</p>
      )}

      {characterCount && (
        <p className="text-sm text-gray-400 text-right">{characterCount}</p>
      )}
    </div>
  );
}
```

Usage:

```jsx
import { Input } from "./components/Input";
import { MagnifyingGlassIcon, PlusIcon } from "@heroicons/react/24/solid";

export default function App() {
  return (
    <div className="space-y-6 p-6 max-w-md mx-auto">
      <Input placeholder="Default input" />

      <Input
        placeholder="With icon left"
        icon={<MagnifyingGlassIcon className="w-5 h-5" />}
      />

      <Input
        placeholder="With icon right"
        iconRight={<PlusIcon className="w-5 h-5" />}
      />

      <Input
        placeholder="Input with error"
        isInvalid
        errorMessage="Error message goes here"
      />

      <Input
        placeholder="With helper text"
        helperText="Some helper text"
      />

      <Input
        placeholder="With counter"
        maxLength={20}
        showCounter
        value="Test"
        onChange={() => {}}
      />

      <Input
        placeholder="With addons"
        leftAddon="Rp"
        rightAddon=".00"
      />
    </div>
  );
}
```
---

### ✅ Custom `Select` Component (Replace all `<select>`)

Use this `Select` component for selections or dropdowns:

```jsx
// components/Select.tsx
import { useState } from 'react'
import { ChevronDownIcon } from 'lucide-react'

export interface Option {
  label: string
  value: string
}

interface SelectProps {
  label?: string
  options: Option[]
  value: string
  onChange: (value: string) => void
  placeholder?: string
}

export function Select({
  label,
  options,
  value,
  onChange,
  placeholder = 'Choose',
}: SelectProps) {
  return (
    <div className="w-full max-w-xs">
      {label && <label className="block mb-2 text-sm font-medium text-gray-700">{label}</label>}
      <div className="relative">
        <select
          className="block w-full appearance-none rounded border border-gray-300 bg-white px-4 py-2 pr-10 text-gray-800 shadow-sm focus:border-blue-500 focus:outline-none focus:ring-1 focus:ring-blue-500"
          value={value}
          onChange={(e) => onChange(e.target.value)}
        >
          <option disabled value="">
            {placeholder}
          </option>
          {options.map((opt) => (
            <option key={opt.value} value={opt.value}>
              {opt.label}
            </option>
          ))}
        </select>
        <div className="pointer-events-none absolute inset-y-0 right-3 flex items-center text-gray-500">
          <ChevronDownIcon className="h-4 w-4" />
        </div>
      </div>
    </div>
  )
}
```

Usage:

```jsx
import { useState } from 'react'
import { Select } from './components/Select'

export default function Page() {
  const [selected, setSelected] = useState('')

  const options = [
    { label: 'yang ada ekornya', value: '1' },
    { label: 'Option 2', value: '2' },
    { label: 'Option 3', value: '3' },
  ]

  return (
    <div className="flex flex-col items-center justify-center min-h-screen gap-6 bg-gray-50 p-6">
      <Select
        label="Select"
        options={options}
        value={selected}
        onChange={setSelected}
        placeholder="Choose"
      />
      <p className="text-sm text-gray-700">selected: {selected}</p>
    </div>
  )
}
```
---

### ✅ Custom `Badge` Component

Use this `Badge` component for all badge elements:

```jsx
// components/Badge.tsx
import clsx from 'clsx'

export interface BadgeProps {
  children: React.ReactNode
  variant?:
    | 'success'
    | 'warning'
    | 'critical'
    | 'informational'
    | 'neutral'
    | 'expressive-success'
    | 'expressive-warning'
    | 'expressive-critical'
    | 'expressive-informational'
    | 'expressive-neutral'
  size?: 'sm' | 'md'
}

export function Badge({ children, variant = 'neutral', size = 'sm' }: BadgeProps) {
  const base = 'inline-flex items-center rounded-full font-medium'
  const sizes = {
    sm: 'text-xs px-2 py-0.5',
    md: 'text-sm px-3 py-1',
  }

  const variants: Record<string, string> = {
    success: 'bg-green-100 text-green-700',
    warning: 'bg-yellow-100 text-yellow-700',
    critical: 'bg-red-100 text-red-700',
    informational: 'bg-blue-100 text-blue-700',
    neutral: 'bg-gray-100 text-gray-700',
    'expressive-success': 'bg-green-600 text-white',
    'expressive-warning': 'bg-yellow-500 text-white',
    'expressive-critical': 'bg-red-600 text-white',
    'expressive-informational': 'bg-blue-600 text-white',
    'expressive-neutral': 'bg-gray-500 text-white',
  }

  return (
    <span className={clsx(base, sizes[size], variants[variant])}>
      {children}
    </span>
  )
}
```

Usage:

```jsx
import { Badge } from './components/Badge'

export default function Page() {
  return (
    <div className="p-6 space-y-6">
      <div className="space-x-2">
        <Badge variant="success">Success</Badge>
        <Badge variant="warning">Warning</Badge>
        <Badge variant="critical">Critical</Badge>
        <Badge variant="informational">Informational</Badge>
        <Badge variant="neutral">Neutral</Badge>
      </div>

      <div className="space-x-2">
        <Badge variant="expressive-success">Success</Badge>
        <Badge variant="expressive-warning">Warning</Badge>
        <Badge variant="expressive-critical">Critical</Badge>
        <Badge variant="expressive-informational">Informational</Badge>
        <Badge variant="expressive-neutral">Neutral</Badge>
      </div>

      <div className="space-y-2">
        <div className="space-x-2">
          <Badge size="sm" variant="success">Small</Badge>
          <Badge size="sm" variant="critical">Small</Badge>
        </div>
        <div className="space-x-2">
          <Badge size="md" variant="success">Medium</Badge>
          <Badge size="md" variant="critical">Medium</Badge>
        </div>
      </div>
    </div>
  )
}
```
---

### ✅ Custom `Table` Component

Use this `Table` component for all table elements:

```jsx
// components/Table.tsx
import { useEffect, useState } from 'react'
import { Input } from '@/components/ui/input' // optional input component
import { ChevronLeft, ChevronRight } from 'lucide-react'

interface TableProps {
  data: any[]
  columns: string[]
  pageSize?: number
}

export function Table({ data, columns, pageSize = 5 }: TableProps) {
  const [search, setSearch] = useState('')
  const [page, setPage] = useState(1)
  const [filtered, setFiltered] = useState(data)

  useEffect(() => {
    const keyword = search.toLowerCase()
    const result = data.filter((item) =>
      columns.some((key) => String(item[key]).toLowerCase().includes(keyword))
    )
    setFiltered(result)
    setPage(1)
  }, [search, data, columns])

  const paginated = filtered.slice((page - 1) * pageSize, page * pageSize)
  const totalPage = Math.ceil(filtered.length / pageSize)

  return (
    <div className="space-y-4">
      <div className="flex justify-between items-center">
        <input
          type="text"
          placeholder="Cari nama sekolah"
          className="border rounded px-3 py-1 text-sm w-64"
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />
      </div>

      <div className="overflow-x-auto border rounded">
        <table className="min-w-full text-sm text-left">
          <thead className="bg-gray-100 text-gray-600">
            <tr>
              {columns.map((col) => (
                <th key={col} className="px-4 py-2 font-medium uppercase">
                  {col}
                </th>
              ))}
            </tr>
          </thead>
          <tbody className="divide-y divide-gray-200">
            {paginated.map((row, idx) => (
              <tr key={idx} className="hover:bg-gray-50">
                {columns.map((col) => (
                  <td key={col} className="px-4 py-2">
                    {row[col]}
                  </td>
                ))}
              </tr>
            ))}
            {paginated.length === 0 && (
              <tr>
                <td colSpan={columns.length} className="text-center py-4 text-gray-500">
                  Data tidak ditemukan
                </td>
              </tr>
            )}
          </tbody>
        </table>
      </div>

      {/* Pagination */}
      <div className="flex justify-between items-center text-sm">
        <span>
          Menampilkan {Math.min((page - 1) * pageSize + 1, filtered.length)} -{' '}
          {Math.min(page * pageSize, filtered.length)} dari {filtered.length} data
        </span>
        <div className="flex items-center gap-1">
          <button
            onClick={() => setPage((p) => Math.max(1, p - 1))}
            className="px-2 py-1 border rounded hover:bg-gray-100 disabled:opacity-50"
            disabled={page === 1}
          >
            <ChevronLeft className="w-4 h-4" />
          </button>
          {Array.from({ length: totalPage }, (_, i) => i + 1).map((p) => (
            <button
              key={p}
              onClick={() => setPage(p)}
              className={`px-3 py-1 rounded ${
                p === page ? 'bg-blue-600 text-white' : 'hover:bg-gray-100 border'
              }`}
            >
              {p}
            </button>
          ))}
          <button
            onClick={() => setPage((p) => Math.min(totalPage, p + 1))}
            className="px-2 py-1 border rounded hover:bg-gray-100 disabled:opacity-50"
            disabled={page === totalPage}
          >
            <ChevronRight className="w-4 h-4" />
          </button>
        </div>
      </div>
    </div>
  )
}
```

Usage:

```jsx
import { Table } from './components/Table'

const columns = ['NPSN', 'NAMA SEKOLAH', 'JUMLAH SISWA', 'ALAMAT', 'STATUS', 'KECAMATAN', 'KODE AKTIVITAS']

const data = [
  {
    NPSN: '20402171',
    'NAMA SEKOLAH': 'SDN TAJUR KIDUL',
    'JUMLAH SISWA': 141,
    ALAMAT: 'Nyampe, Sidaraja',
    STATUS: 'Negeri',
    KECAMATAN: 'Kec. Panjeng',
    'KODE AKTIVITAS': 'NF08',
  },
  {
    NPSN: '20402172',
    'NAMA SEKOLAH': 'SDN BULUREJO 1',
    'JUMLAH SISWA': 112,
    ALAMAT: 'Karanggub',
    STATUS: 'Negeri',
    KECAMATAN: 'Kec. Semih',
    'KODE AKTIVITAS': '107J',
  },
  // ... more rows
]

export default function Page() {
  return (
    <main className="p-6">
      <h1 className="text-lg font-bold mb-4">DataTable</h1>
      <Table data={data} columns={columns} pageSize={5} />
    </main>
  )
}
```
---

### ✅ Custom `Textarea` Component

Use this `Textarea` component for all textarea elements:

```jsx
// components/Textarea.tsx
import * as React from 'react'
import clsx from 'clsx'

export interface TextareaProps extends React.TextareaHTMLAttributes<HTMLTextAreaElement> {
  isDisabled?: boolean
  isInvalid?: boolean
  helperText?: string
  errorMessage?: string
  maxLength?: number
  counterSlot?: ({
    counter,
    text,
  }: {
    counter: number
    text: string
  }) => React.ReactNode
}

export const Textarea = React.forwardRef<HTMLTextAreaElement, TextareaProps>(
  (
    {
      className,
      isDisabled = false,
      isInvalid = false,
      helperText,
      errorMessage,
      maxLength,
      value,
      onChange,
      counterSlot,
      ...props
    },
    ref
  ) => {
    const [inputLength, setInputLength] = React.useState(0)
    const internalRef = React.useRef<HTMLTextAreaElement>(null)

    const handleChange = (e: React.ChangeEvent<HTMLTextAreaElement>) => {
      setInputLength(e.target.value.length)
      onChange?.(e)
    }

    const assignRef = (node: HTMLTextAreaElement | null) => {
      internalRef.current = node
      if (typeof ref === 'function') {
        ref(node)
      } else if (ref) {
        ;(ref as React.RefObject<HTMLTextAreaElement>).current = node
      }
    }

    React.useEffect(() => {
      const val = internalRef.current?.value || ''
      setInputLength(val.length)
    }, [])

    return (
      <div className="w-full space-y-1">
        <textarea
          ref={assignRef}
          disabled={isDisabled}
          aria-disabled={isDisabled}
          aria-invalid={isInvalid}
          className={clsx(
            'w-full resize-none rounded border px-4 py-3 text-sm transition-colors focus:outline-none',
            'placeholder:text-gray-400',
            {
              'border-gray-300 focus:border-blue-500 focus:ring-2 focus:ring-blue-200': !isInvalid && !isDisabled,
              'border-red-500 bg-red-50 text-red-900 focus:ring-red-200': isInvalid,
              'bg-gray-100 text-gray-500 border-gray-200 cursor-not-allowed': isDisabled,
            },
            className
          )}
          rows={5}
          maxLength={maxLength}
          value={value}
          onChange={handleChange}
          {...props}
        />

        {(helperText || errorMessage || maxLength || counterSlot) && (
          <div className="flex justify-between text-xs mt-1 text-gray-500">
            <span className={clsx({ 'text-red-600': isInvalid })}>
              {errorMessage || helperText}
            </span>
            <span className={clsx({ 'text-red-600': isInvalid })}>
              {typeof maxLength === 'number'
                ? `${inputLength}/${maxLength}`
                : counterSlot?.({
                    counter: inputLength,
                    text: internalRef.current?.value || '',
                  })}
            </span>
          </div>
        )}
      </div>
    )
  }
)

Textarea.displayName = 'Textarea'
```

Usage:

```jsx
import { useState } from 'react'
import { Textarea } from './components/Textarea'

export default function Page() {
  const [value, setValue] = useState('')

  return (
    <div className="p-6 max-w-xl mx-auto">
      <h1 className="text-lg font-semibold mb-4">Custom Textarea</h1>
      <Textarea
        value={value}
        onChange={(e) => setValue(e.target.value)}
        placeholder="Tulis pesan di sini..."
        maxLength={120}
        helperText="Bisa diisi hingga 120 karakter"
      />
    </div>
  )
}
```
---

### ✅ Custom `Text` Component

Use this `Text` component for all text elements:

```jsx
// components/Text.tsx
import * as React from 'react'
import clsx from 'clsx'
import { Primitive } from '@wartek-id/react-primitive'
import type * as Polymorphic from '@wartek-id/react-polymorphic'

const TEXT_VARIANTS = {
  helper: 'text-xs font-normal',
  'helper-bold': 'text-xs font-semibold',
  'body-sm': 'text-sm font-normal',
  'body-sm-bold': 'text-sm font-semibold',
  body: 'text-base font-normal',
  'body-bold': 'text-base font-semibold',
  'body-lg': 'text-lg font-normal',
  'body-lg-bold': 'text-lg font-semibold',
  action: 'text-sm font-medium',
  'action-secondary': 'text-sm font-medium italic',
  subheading: 'text-lg font-medium',
  'heading-sm': 'text-xl font-semibold',
  'heading-md': 'text-2xl font-semibold',
  'heading-lg': 'text-3xl font-semibold',
  'display-sm': 'text-4xl font-bold',
  'display-md': 'text-5xl font-bold',
  'display-lg': 'text-6xl font-bold',
  'eyebrow-sm': 'text-[10px] uppercase tracking-wider',
  'eyebrow-md': 'text-[12px] uppercase tracking-wider',
  'eyebrow-lg': 'text-[14px] uppercase tracking-wider',
  'headline-xs': 'text-[18px] font-bold',
  'headline-sm': 'text-[20px] font-bold',
  'headline-md': 'text-[24px] font-bold',
  'headline-lg': 'text-[28px] font-bold',
}

const TEXT_COLORS = {
  default: 'text-neutral-900',
  subdued: 'text-neutral-600',
  disabled: 'text-neutral-400',
  critical: 'text-red-600',
  inverse: 'text-white',
  'inverse-subdued': 'text-white/80',
  warning: 'text-yellow-600',
  success: 'text-green-600',
  informational: 'text-blue-600',
  primary: 'text-primary',
  inherit: 'text-inherit',
}

const DEFAULT_TAG = 'p'

export interface TextProps {
  variant?: keyof typeof TEXT_VARIANTS
  color?: keyof typeof TEXT_COLORS
  className?: string
}

type TextOwnProps = Polymorphic.OwnProps<typeof Primitive> & TextProps
type TextPrimitive = Polymorphic.ForwardRefComponent<typeof DEFAULT_TAG, TextOwnProps>

export const Text = React.forwardRef((props, ref) => {
  const {
    as = DEFAULT_TAG,
    variant = 'body',
    color = 'default',
    className,
    ...textProps
  } = props

  const variantClass = TEXT_VARIANTS[variant] || ''
  const colorClass = TEXT_COLORS[color] || ''

  return (
    <Primitive
      as={as}
      ref={ref}
      className={clsx(variantClass, colorClass, className)}
      {...textProps}
    />
  )
}) as TextPrimitive

Text.displayName = 'Text'
```

Usage:

```jsx
import { Text } from './components/Text'

export default function ExamplePage() {
  return (
    <div className="space-y-4">
      <Text variant="headline-md" color="primary">
        Headline Example
      </Text>
      <Text variant="body" color="default">
        This is a body text with default color.
      </Text>
      <Text variant="body-sm-bold" color="subdued">
        Small bold text in subdued color.
      </Text>
      <Text variant="eyebrow-sm" color="critical">
        EYEBROW LABEL
      </Text>
      <Text variant="display-lg" color="success">
        Big Display Text
      </Text>
    </div>
  )
}
```
---

### ✅ Custom `Checkbox` Component

Use this `Checkbox` component for all checkbox elements:

```jsx
// components/Checkbox.tsx
import * as React from 'react'
import clsx from 'clsx'

export interface CheckboxProps {
  value?: string
  children?: React.ReactNode
  checked?: boolean
  indeterminate?: boolean
  disabled?: boolean
  invalid?: boolean
  name?: string
  className?: string
  controlled?: boolean
  position?: 'normal' | 'reverse'
  required?: boolean
  onChange?: (event: React.ChangeEvent<HTMLInputElement>) => void
  style?: React.CSSProperties
  labelPosition?: 'start' | 'end' | 'center'
}

export const Checkbox = React.forwardRef<HTMLInputElement, CheckboxProps>(
  (
    {
      value = '',
      name,
      disabled,
      checked,
      invalid,
      indeterminate,
      children,
      className,
      onChange,
      style,
      position = 'normal',
      required = false,
      controlled = false,
      labelPosition = 'center',
      ...checkboxProps
    },
    ref
  ) => {
    const [isChecked, setIsChecked] = React.useState(checked)

    const internalCheckRef = React.useRef<HTMLInputElement>(null)

    React.useEffect(() => {
      if (internalCheckRef.current) {
        internalCheckRef.current.indeterminate = !!indeterminate
      }
    }, [indeterminate])

    React.useEffect(() => {
      setIsChecked(checked)
    }, [checked])

    function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
      if (!controlled) {
        setIsChecked(event.target.checked)
      }
      onChange?.(event)
    }

    return (
      <label
        className={clsx(
          'inline-flex items-center cursor-pointer select-none',
          position === 'reverse' && 'flex-row-reverse justify-end',
          labelPosition === 'start' && 'items-start',
          labelPosition === 'end' && 'items-end',
          labelPosition === 'center' && 'items-center',
          disabled && 'cursor-not-allowed opacity-60',
          className
        )}
        style={style}
      >
        <input
          ref={(el) => {
            internalCheckRef.current = el
            if (typeof ref === 'function') ref(el)
          }}
          type="checkbox"
          name={name}
          value={value}
          checked={!!isChecked}
          disabled={disabled}
          aria-required={required}
          aria-invalid={invalid}
          onChange={handleChange}
          className="sr-only"
          {...checkboxProps}
        />
        <span
          className={clsx(
            'w-[18px] h-[18px] rounded-md flex items-center justify-center border border-gray-400 bg-white transition-all duration-200',
            isChecked &&
              'bg-blue-600 border-blue-600 bg-[url("data:image/svg+xml,%3Csvg width=\'14\' height=\'14\' viewBox=\'0 0 14 14\' fill=\'none\' xmlns=\'http://www.w3.org/2000/svg\'%3E%3Cpath d=\'M4.75 10.13L1.62 7 0.56 8.06 4.75 12.25 13.75 3.25 12.69 2.19 4.75 10.13Z\' fill=\'%23F5F5F5\'/%3E%3C/svg%3E")]',
            indeterminate &&
              'bg-blue-600 border-blue-600 bg-[url("data:image/svg+xml,%3Csvg xmlns=\'http://www.w3.org/2000/svg\' height=\'20px\' viewBox=\'0 0 24 24\' width=\'20px\' fill=\'%23f5f5f5\'%3E%3Cpath d=\'M19 13H5v-2h14v2z\'/%3E%3C/svg%3E")]',
            disabled && 'bg-gray-200 border-gray-300'
          )}
        />
        {children && (
          <span className={clsx('ml-2 text-sm text-gray-900')}>
            {children}
          </span>
        )}
      </label>
    )
  }
)

Checkbox.displayName = 'Checkbox'
```

Usage:

```jsx
import { Checkbox } from './components/Checkbox'

export default function Example() {
  return (
    <div className="space-y-4">
      <Checkbox>Default Checkbox</Checkbox>

      <Checkbox checked>Checked Checkbox</Checkbox>

      <Checkbox indeterminate>Indeterminate Checkbox</Checkbox>

      <Checkbox disabled>Disabled Checkbox</Checkbox>

      <Checkbox position="reverse">Reverse Position</Checkbox>

      <Checkbox labelPosition="start">Start Label</Checkbox>

      <Checkbox labelPosition="end">End Label</Checkbox>

      <Checkbox labelPosition="center" position="reverse">
        Reverse & Centered Label
      </Checkbox>
    </div>
  )
}
```
---

### ✅ Custom `Radio` Component

Use this `Radio` component for all radio button elements:

```jsx
// components/Radio.tsx
// RadioContext.ts
import { createContext } from '@wartek-id/react-utils'
import type { RadioGroupProps } from './RadioGroup'

type RadioContextType = Omit<RadioGroupProps, 'children' | 'className' | 'style'>

export const [RadioContextProvider, useRadioContext] =
  createContext<RadioContextType>({
    name: 'RadioGroupContext',
    strict: false,
  })

// RadioGroup.tsx
import * as React from 'react'
import clsx from 'clsx'
import { RadioContextProvider } from './RadioContext'

export interface RadioGroupProps {
  children: React.ReactNode
  value?: string
  onChange?: (value: string) => void
  name: string
  className?: string
  style?: React.CSSProperties
  labelPosition?: 'start' | 'end' | 'center'
}

export const RadioGroup = (props: RadioGroupProps) => {
  const {
    value,
    name,
    children,
    onChange,
    className,
    labelPosition = 'start',
    ...radioGroupProps
  } = props

  return (
    <RadioContextProvider value={{ value, name, onChange, labelPosition }}>
      <div
        className={clsx('flex flex-col gap-2', className)}
        role="radiogroup"
        {...radioGroupProps}
      >
        {children}
      </div>
    </RadioContextProvider>
  )
}

RadioGroup.displayName = 'RadioGroup'

// Radio.tsx
import * as React from 'react'
import clsx from 'clsx'
import { useRadioContext } from './RadioContext'

export interface RadioProps extends React.HTMLAttributes<HTMLLabelElement> {
  value: string
  children: React.ReactNode
  checked?: boolean
  disabled?: boolean
  name?: string
  labelPosition?: 'start' | 'end' | 'center'
}

export const Radio = React.forwardRef((props: RadioProps, radioRef: any) => {
  const {
    value,
    disabled,
    name,
    checked,
    children,
    className,
    labelPosition,
    ...radioProps
  } = props

  const context = useRadioContext()
  const groupName = context?.name || name
  const isChecked = context?.value === value || checked
  const labelAlign = labelPosition || context?.labelPosition || 'start'

  return (
    <label
      className={clsx(
        'inline-flex items-center gap-2 cursor-pointer',
        labelAlign === 'start' && 'items-start',
        labelAlign === 'center' && 'items-center',
        labelAlign === 'end' && 'items-end',
        disabled && 'cursor-not-allowed opacity-60',
        className
      )}
      aria-disabled={disabled}
      {...radioProps}
    >
      <input
        ref={radioRef}
        type="radio"
        className="sr-only"
        name={groupName}
        value={value}
        checked={isChecked}
        disabled={disabled}
        onChange={(e) => context?.onChange?.(e.target.value)}
      />
      <span
        className={clsx(
          'w-5 h-5 rounded-full border border-gray-400 flex items-center justify-center transition-all',
          isChecked && 'border-4 border-blue-600',
          disabled && 'border-gray-300 bg-gray-100'
        )}
      />
      <span className="text-sm text-gray-900">{children}</span>
    </label>
  )
})

Radio.displayName = 'Radio'

export * from './RadioGroup'
export * from './RadioContext'
```

Usage:

```jsx
import { RadioGroup, Radio } from './components/Radio'

export default function Example() {
  const [value, setValue] = React.useState('option-1')

  return (
    <RadioGroup name="example-group" value={value} onChange={setValue}>
      <Radio value="option-1">Option 1</Radio>
      <Radio value="option-2">Option 2</Radio>
      <Radio value="option-3" disabled>
        Disabled Option
      </Radio>
    </RadioGroup>
  )
}
```
---

### ✅ Custom `Dialog` Component

Use this `Dialog` component for all modal dialogs:

```jsx
// components/Dialog.jsx
import { useEffect } from "react";

export function Dialog({ open, onClose, onConfirm }) {
  useEffect(() => {
    const handleEsc = (e) => e.key === "Escape" && onClose();
    if (open) document.addEventListener("keydown", handleEsc);
    return () => document.removeEventListener("keydown", handleEsc);
  }, [open, onClose]);

  if (!open) return null;

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/40">
      <div className="bg-white rounded shadow-lg w-full max-w-md">
        <div className="flex justify-between items-center border-b px-4 py-3">
          <h2 className="text-sm font-semibold">Hapus user John dari List?</h2>
          <button onClick={onClose} className="text-xl leading-none">&times;</button>
        </div>
        <div className="px-4 py-3 text-sm">
          User <strong>John</strong> akan dihapus dari list. Apakah kamu yakin?
        </div>
        <div className="flex justify-end gap-2 px-4 py-3 border-t">
          <button
            onClick={onClose}
            className="px-4 py-1 text-sm border rounded hover:bg-gray-50"
          >
            Nah!
          </button>
          <button
            onClick={onConfirm}
            className="px-4 py-1 text-sm bg-black text-white rounded hover:bg-gray-800"
          >
            Sure, delete him!
          </button>
        </div>
      </div>
    </div>
  );
}
```

Usage:

```jsx
import { useState } from "react";
import { Dialog } from "./components/Dialog";

export default function DemoPage() {
  const [isDialogOpen, setIsDialogOpen] = useState(false);

  const handleDelete = () => {
    alert("User deleted!");
    setIsDialogOpen(false);
  };

  return (
    <div className="min-h-screen bg-gray-50 p-6">
      <h1 className="text-xl font-bold mb-6">Tailwind Component Demo</h1>

      <button
        onClick={() => setIsDialogOpen(true)}
        className="px-4 py-2 bg-blue-600 text-white rounded hover:bg-blue-700"
      >
        Hapus User
      </button>

      <Dialog
        open={isDialogOpen}
        onClose={() => setIsDialogOpen(false)}
        onConfirm={handleDelete}
      />
    </div>
  );
}
```
---

### ✅ Custom `Breadcrumb` Component

Use this `Breadcrumb` component for all breadcrumb navigation:

```jsx
// components/Breadcrumb.jsx
import { ChevronRight } from 'lucide-react';

export function Breadcrumb({ items = [] }) {
  return (
    <nav className="text-sm" aria-label="Breadcrumb">
      <ol className="flex items-center gap-1 text-gray-500">
        {items.map((item, idx) => (
          <li key={idx} className="flex items-center">
            {idx !== 0 && (
              <ChevronRight className="w-4 h-4 text-gray-400 mx-1" />
            )}
            {item.href ? (
              <a
                href={item.href}
                className="text-blue-600 hover:underline font-medium"
              >
                {item.label}
              </a>
            ) : (
              <span className="text-gray-900 font-semibold">{item.label}</span>
            )}
          </li>
        ))}
      </ol>
    </nav>
  );
}
```

Usage:

```jsx
import { Breadcrumb } from './components/Breadcrumb';

export default function Page() {
  return (
    <div className="p-6">
      <Breadcrumb
        items={[
          { label: 'Dashboard', href: '/' },
          { label: 'Penilaian Periodik', href: '/penilaian-periodik' },
          { label: 'Form Penilaian' },
        ]}
      />

      <h1 className="text-2xl font-bold mt-4">Form Penilaian</h1>
    </div>
  );
}
```
---

### ✅ Custom `Tabs` Component

Use this `Tabs` component for all tab navigation:

```jsx
// components/Tabs.jsx
import { useState } from 'react';

export function Tabs({ tabs = [], defaultIndex = 0, withBorder = false, isFitted = false, onChange }) {
  const [selectedIndex, setSelectedIndex] = useState(defaultIndex);

  const handleClick = (index) => {
    setSelectedIndex(index);
    if (onChange) onChange(index);
  };

  return (
    <div>
      <div
        className={`flex ${isFitted ? 'justify-between' : 'gap-4'} border-b ${withBorder ? 'border-gray-200' : ''}`}
      >
        {tabs.map((tab, index) => (
          <button
            key={index}
            onClick={() => handleClick(index)}
            className={`py-2 px-4 text-sm font-medium whitespace-nowrap transition-colors duration-200
              ${selectedIndex === index
                ? 'text-blue-600 border-b-2 border-blue-600'
                : 'text-gray-600 hover:text-blue-600'}
              ${isFitted ? 'flex-1 text-center' : ''}`}
          >
            {tab.label}
          </button>
        ))}
      </div>
      <div className="mt-4 text-sm text-gray-800">{tabs[selectedIndex]?.content}</div>
    </div>
  );
}
```

Usage:

```jsx
import { Tabs } from './components/Tabs';

export default function Page() {
  return (
    <div className="p-6 max-w-xl mx-auto">
      <Tabs
        defaultIndex={0}
        withBorder={true}
        isFitted={false}
        tabs={[
          { label: 'Kampus Merdeka', content: 'Kampus Merdeka adalah …' },
          { label: 'Guru Mengajar', content: 'Guru Mengajar adalah …' },
          { label: 'Guru Portfolio', content: 'Guru Portfolio adalah …' },
        ]}
      />
    </div>
  );
}
```
---

### ✅ Custom `FileUpload` Component

Use this `FileUpload` component for file upload functionality:

```jsx
// components/FileUpload.jsx
import React, { useRef, useState } from "react";
import { PaperclipIcon } from "@heroicons/react/24/outline";

export default function FileUpload() {
  const fileInputRef = useRef(null);
  const [file, setFile] = useState(null);

  const handleFileChange = (e) => {
    const selectedFile = e.target.files?.[0];
    if (selectedFile) {
      setFile(selectedFile);
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!file) return alert("Please upload a file.");
    // You can handle upload logic here
    console.log("Uploading file:", file);
  };

  return (
    <form onSubmit={handleSubmit} className="max-w-sm w-full space-y-3">
      <label className="block text-sm font-medium text-gray-800">
        Unggah Dokumen
      </label>
      <p className="text-xs text-gray-500">Ukuran maksimal 1mb</p>

      <div
        className="border border-dashed border-gray-300 rounded-md px-4 py-3 flex items-center gap-2 text-gray-600 cursor-pointer"
        onClick={() => fileInputRef.current?.click()}
      >
        <PaperclipIcon className="w-4 h-4" />
        <span className="text-sm">
          {file ? file.name : "Pilih File"}
        </span>
      </div>

      <input
        ref={fileInputRef}
        type="file"
        className="hidden"
        onChange={handleFileChange}
      />

      <button
        type="submit"
        className="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-2 px-4 rounded-md text-sm"
      >
        Submit
      </button>
    </form>
  );
}
```

Usage:

```jsx
import FileUpload from "./components/FileUpload";

export default function UploadPage() {
  return (
    <div className="min-h-screen flex items-center justify-center bg-gray-50 p-6">
      <FileUpload />
    </div>
  );
}
```
---

### ✅ Custom `Text` Component

Use this `Text` component for file upload functionality:

```jsx
// components/Text.jsx
import clsx from "clsx";

const variantClasses = {
  body: "text-base",
  action: "text-base font-medium uppercase tracking-wide",
  helper: "text-sm text-gray-500",
  "helper-bold": "text-sm font-semibold text-gray-500",
  "body-sm": "text-sm",
  "body-sm-bold": "text-sm font-semibold",
  "body-bold": "text-base font-semibold",
  "body-lg": "text-lg",
  "body-lg-bold": "text-lg font-bold",
  "action-secondary": "text-sm uppercase tracking-wide text-gray-500",
  subheading: "text-lg font-medium text-gray-700",

  "heading-sm": "text-xl font-semibold",
  "heading-md": "text-2xl font-semibold",
  "heading-lg": "text-3xl font-bold",

  "display-sm": "text-4xl font-bold",
  "display-md": "text-5xl font-bold",
  "display-lg": "text-6xl font-bold",

  "eyebrow-sm": "text-xs uppercase tracking-wide text-gray-500",
  "eyebrow-md": "text-sm uppercase tracking-wide text-gray-500",
  "eyebrow-lg": "text-base uppercase tracking-wide text-gray-500",

  "headline-xs": "text-lg font-semibold",
  "headline-sm": "text-xl font-semibold",
  "headline-md": "text-2xl font-semibold",
  "headline-lg": "text-3xl font-semibold",
};

const colorClasses = {
  default: "text-gray-900",
  inherit: "text-inherit",
  disabled: "text-gray-400",
  success: "text-green-600",
  subdued: "text-gray-500",
  critical: "text-red-600",
  inverse: "text-white",
  "inverse-subdued": "text-white/70",
  warning: "text-yellow-600",
  informational: "text-blue-600",
  primary: "text-primary", // define in tailwind.config.js
};

export function Text({
  variant = "body",
  color = "default",
  as = "p",
  className,
  children,
  ...props
}) {
  const Component = as;

  return (
    <Component
      className={clsx(
        variantClasses[variant],
        colorClasses[color],
        className
      )}
      {...props}
    >
      {children}
    </Component>
  );
}
```

Usage:

```jsx
import { Text } from "./components/Text";

export default function Demo() {
  return (
    <div className="space-y-4">
      <Text variant="heading-lg" color="primary">Heading Large</Text>
      <Text variant="display-md" color="informational">Display Medium</Text>
      <Text variant="body-sm-bold" color="subdued">Small Bold Body</Text>
      <Text variant="eyebrow-md" color="warning">Warning Tag</Text>
      <Text variant="helper" color="disabled">This is a helper text</Text>
    </div>
  );
}
```

---

### ✅ Custom `Icon` Component

Use this `Icon` component for all icon elements:

```jsx
// components/Icon.tsx
import * as React from 'react'
import { cn } from '@/lib/utils'

export interface IconProps extends React.HTMLAttributes<HTMLSpanElement> {
  color?:
    | 'default'
    | 'disabled'
    | 'critical'
    | 'warning'
    | 'success'
    | 'informational'
    | 'inverse'
    | 'inherit'
  fontSize?: 'default' | 'sm' | 'lg'
  variant?: 'filled' | 'outlined'
}

export const Icon = React.forwardRef<HTMLSpanElement, IconProps>(
  (
    {
      color = 'default',
      fontSize = 'default',
      variant = 'filled',
      className,
      children,
      ...props
    },
    ref
  ) => {
    const variantClass =
      variant === 'outlined' ? 'material-icons-outlined' : 'material-icons'

    const sizeClasses = {
      default: 'text-[24px] w-6 h-6',
      sm: 'text-[20px] w-5 h-5',
      lg: 'text-[36px] w-9 h-9',
    }

    const colorClasses: Record<string, string> = {
      default: 'text-neutral-700',
      disabled: 'text-neutral-400',
      critical: 'text-red-600',
      warning: 'text-orange-500',
      success: 'text-green-600',
      informational: 'text-blue-600',
      inverse: 'text-white',
      inherit: 'text-inherit',
    }

    return (
      <span
        ref={ref}
        className={cn(
          variantClass,
          sizeClasses[fontSize],
          colorClasses[color],
          className
        )}
        role="img"
        aria-hidden="true"
        {...props}
      >
        {children}
      </span>
    )
  }
)

Icon.displayName = 'Icon'
```

Usage:

```jsx
import { Icon } from './components/Icon';

export default function Page() {
  return (
    <>
      <Icon>home</Icon>

      <Icon color="critical" fontSize="lg">delete</Icon>

      <Icon variant="outlined" color="informational" fontSize="sm">info</Icon>

    </>
  );
}
```

Required Setup:
1. Add Google Fonts Icon Link (in _document.tsx or index.html):
```html
<link
  href="https://fonts.googleapis.com/icon?family=Material+Icons|Material+Icons+Outlined"
  rel="stylesheet"
/>
```

2. Optional: Add cn helper (if not using clsx):
```javascript
// lib/utils.ts
export function cn(...args: any[]) {
  return args.filter(Boolean).join(' ')
}
```
---

## Color Guides

This guide provides a set of color palettes for Vercel v0, designed using Tailwind CSS classes with corresponding HEX codes based on the reference palette.

### Gray
- 5: `bg-gray-50` (#F9FAFB)
- 10: `bg-gray-100` (#F4F5F7)
- 20: `bg-gray-200` (#EDEFF2)
- 30: `bg-gray-300` (#E2E4E9)
- 40: `bg-gray-400` (#D6D9E0)
- 50: `bg-gray-500` (#A9B0C0)
- 60: `bg-gray-600` (#7D8BA6)
- 70: `bg-gray-700` (#5A6A87)
- 80: `bg-gray-800` (#3A4B6A)
- 90: `bg-gray-900` (#1F2A44)
- 100: `bg-gray-950` (#0D172A)

### Red
- 5: `bg-red-50` (#FFF1F3)
- 10: `bg-red-100` (#FFDCE0)
- 20: `bg-red-200` (#FFC7CD)
- 30: `bg-red-300` (#FF9FA9)
- 40: `bg-red-400` (#FF7784)
- 50: `bg-red-500` (#EF2A4A)
- 60: `bg-red-600` (#D91536)
- 70: `bg-red-700` (#B70C25)
- 80: `bg-red-800` (#970A1E)
- 90: `bg-red-900` (#5F0713)
- 100: `bg-red-950` (#2F040A)

### Pink
- 5: `bg-pink-50` (#FDF2F8)
- 10: `bg-pink-100` (#FCE7F3)
- 20: `bg-pink-200` (#FAD1E8)
- 30: `bg-pink-300` (#F6A8D1)
- 40: `bg-pink-400` (#EF7DBF)
- 50: `bg-pink-500` (#E74694)
- 60: `bg-pink-600` (#D61F7E)
- 70: `bg-pink-700` (#BB0E6A)
- 80: `bg-pink-800` (#9F0C5A)
- 90: `bg-pink-900` (#61073A)
- 100: `bg-pink-950` (#2F031D)

### Purple
- 5: `bg-purple-50` (#F9F5FF)
- 10: `bg-purple-100` (#F3EFFF)
- 20: `bg-purple-200` (#E6D7FE)
- 30: `bg-purple-300` (#D4BBFC)
- 40: `bg-purple-400` (#C298FA)
- 50: `bg-purple-500` (#A855F7)
- 60: `bg-purple-600` (#9333EA)
- 70: `bg-purple-700` (#7A22DF)
- 80: `bg-purple-800` (#651BBB)
- 90: `bg-purple-900` (#3F0C90)
- 100: `bg-purple-950` (#1E0449)

### Blue
- 5: `bg-blue-50` (#EFF6FF)
- 10: `bg-blue-100` (#DBEAFE)
- 20: `bg-blue-200` (#BFDBFE)
- 30: `bg-blue-300` (#93C5FD)
- 40: `bg-blue-400` (#60A5FA)
- 50: `bg-blue-500` (#3B82F6)
- 60: `bg-blue-600` (#2563EB)
- 70: `bg-blue-700` (#1D4ED8)
- 80: `bg-blue-800` (#1E40AF)
- 90: `bg-blue-900` (#1E2A80)
- 100: `bg-blue-950` (#172554)

### Cyan
- 5: `bg-cyan-50` (#F0FDFC)
- 10: `bg-cyan-100` (#CCF7F5)
- 20: `bg-cyan-200` (#A5F0EC)
- 30: `bg-cyan-300` (#67E2DA)
- 40: `bg-cyan-400` (#22D3C5)
- 50: `bg-cyan-500` (#06B6CC)
- 60: `bg-cyan-600` (#0891B2)
- 70: `bg-cyan-700` (#0E7490)
- 80: `bg-cyan-800` (#0F5575)
- 90: `bg-cyan-900` (#0D3A58)
- 100: `bg-cyan-950` (#0A2C3F)

### Teal
- 5: `bg-teal-50` (#F0FDFA)
- 10: `bg-teal-100` (#CCFBF1)
- 20: `bg-teal-200` (#99F6E0)
- 30: `bg-teal-300` (#5EEAD4)
- 40: `bg-teal-400` (#2DD4BF)
- 50: `bg-teal-500` (#14B8A6)
- 60: `bg-teal-600` (#0D9488)
- 70: `bg-teal-700` (#0F766E)
- 80: `bg-teal-800` (#115E59)
- 90: `bg-teal-900` (#0D4A45)
- 100: `bg-teal-950` (#0A2F2E)

### Green
- 5: `bg-green-50` (#F0FDF4)
- 10: `bg-green-100` (#CCFCE7)
- 20: `bg-green-200` (#A7F3D0)
- 30: `bg-green-300` (#6EE7B7)
- 40: `bg-green-400` (#34D399)
- 50: `bg-green-500` (#10B981)
- 60: `bg-green-600` (#059669)
- 70: `bg-green-700` (#047857)
- 80: `bg-green-800` (#065F46)
- 90: `bg-green-900` (#064E3B)
- 100: `bg-green-950` (#022C22)

### Orange
- 5: `bg-orange-50` (#FFF7ED)
- 10: `bg-orange-100` (#FFEDD5)
- 20: `bg-orange-200` (#FED7AA)
- 30: `bg-orange-300` (#FDBA74)
- 40: `bg-orange-400` (#FB923C)
- 50: `bg-orange-500` (#F97316)
- 60: `bg-orange-600` (#EA580C)
- 70: `bg-orange-700` (#C2410C)
- 80: `bg-orange-800` (#9A3412)
- 90: `bg-orange-900` (#5F1F07)
- 100: `bg-orange-950` (#2F0F03)

### Yellow
- 5: `bg-yellow-50` (#FEFCE8)
- 10: `bg-yellow-100` (#FEF9C3)
- 20: `bg-yellow-200` (#FEF08A)
- 30: `bg-yellow-300` (#FDE047)
- 40: `bg-yellow-400` (#FACC15)
- 50: `bg-yellow-500` (#EAB308)
- 60: `bg-yellow-600` (#CA8A04)
- 70: `bg-yellow-700` (#A16207)
- 80: `bg-yellow-800` (#854D0E)
- 90: `bg-yellow-900` (#5A3408)
- 100: `bg-yellow-950` (#2F1A05)

### Tropicana
- 5: `bg-lime-50` (#F7FEE7)
- 10: `bg-lime-100` (#ECFCCB)
- 20: `bg-lime-200` (#D9F99D)
- 30: `bg-lime-300` (#BEF264)
- 40: `bg-lime-400` (#A3E635)
- 50: `bg-lime-500` (#84CC16)
- 60: `bg-lime-600` (#65A30D)
- 70: `bg-lime-700` (#4D7C0F)
- 80: `bg-lime-800` (#3F6212)
- 90: `bg-lime-900` (#2A4408)
- 100: `bg-lime-950` (#142204)

### Indigo
- 5: `bg-indigo-50` (#F0F2FF)
- 10: `bg-indigo-100` (#E0E7FF)
- 20: `bg-indigo-200` (#C7D2FE)
- 30: `bg-indigo-300` (#A5B4FC)
- 40: `bg-indigo-400` (#818CF8)
- 50: `bg-indigo-500` (#6366F1)
- 60: `bg-indigo-600` (#4F46E5)
- 70: `bg-indigo-700` (#4338CA)
- 80: `bg-indigo-800` (#3730A3)
- 90: `bg-indigo-900` (#2B2A79)
- 100: `bg-indigo-950` (#1A1B3F)

---

## 📌 Mandatory Layout Code

You must wrap each generated pages inside the `Layout` component defined in the following JSX code. Do **not** modify the layout itself — only insert your page-specific content inside the `<main>{children}</main>` area of the layout.
```jsx
// components/Layout.jsx
import React from "react";
import {
  HomeIcon,
  UserIcon,
  BellIcon,
  QuestionMarkCircleIcon,
  ChevronDownIcon,
  LockClosedIcon,
} from "@heroicons/react/24/outline";

const menuItems = [
  { label: "Beranda", icon: <HomeIcon className="w-6 h-6" /> },
  { label: "Feature Name", icon: <UserIcon className="w-6 h-6" />, active: true },
  { label: "Notifikasi", icon: <BellIcon className="w-6 h-6" />, badge: 168 },
  { label: "Hubungi Kami", icon: <QuestionMarkCircleIcon className="w-6 h-6" /> },
];

export default function Layout({
  children
}) {
  return (
    <div className="flex min-h-screen bg-gray-100">
      {/* Sidebar */}
      <aside className="w-64 bg-white border-r shadow-sm flex-shrink-0">
        <div className="px-6 py-4 flex items-center gap-2">
          <img src="/logo.png" alt="Logo" className="w-8 h-8" />
          <span className="font-bold text-lg">App Name</span>
        </div>
        <nav className="mt-6">
          {menuItems.map((item) => (
            <div
              key={item.label}
              className={`px-6 py-3 flex items-center gap-3 cursor-pointer ${
                item.active ? "bg-blue-50 font-semibold border-l-4 border-blue-600" : "text-gray-700 hover:bg-gray-50"
              }`}
            >
              {item.icon}
              <span className="flex-1">{item.label}</span>
              {item.badge && (
                <span className="text-xs bg-blue-600 text-white rounded-full px-2">
                  {item.badge}
                </span>
              )}
            </div>
          ))}
        </nav>
      </aside>

      {/* Main Content */}
      <div className="flex-1 flex flex-col">
        {/* Header */}
        <header className="h-16 bg-white border-b flex items-center justify-end px-6">
          <div className="flex items-center gap-2 cursor-pointer">
            <UserIcon className="w-6 h-6 text-gray-600" />
            <span className="text-gray-700 font-medium">User</span>
            <ChevronDownIcon className="w-5 h-5 text-gray-500" />
          </div>
        </header>

        {/* Main Layout */}
        <main className="flex-1 p-6 space-y-4">
          {children}
        </main>
      </div>
    </div>
  );
}
```

### Example Usage:
```jsx
import Layout from "./components/Layout";

export default function MyPage() {
  return (
    <Layout>
      {/* Add your page content here */}
    </Layout>
  );
}
```

---
