# 0. Intro / Initial Styling
```
npm create vite@latest
```
- select uses `defaultValue` instead of selected
# 1. Multiple States Form
- create a state for each input
- add a value and `onChange` attribute to each input
- remove `defaultValue` for select

- show react developer tools
- resetting

# 2. Single State Form
- create a shared state for all inputs
- add a value and `onChange` attribute to each input
- remove `defaultValue` for select

- add name property to each input
- reusable click handler
- resetting

# 3. Custom Form Hook
- create `useForm` hook
- add name property to each input
- create `handleChange` type

# 4. Form Data Form
- add name attribute (add to base form since I will be using it in Formik videos) to each form
- `Object.fromEntries`
```
const form = e.target as HTMLFormElement;
const formData = Object.fromEntries(new FormData(form));
```

```
const form = e.target as HTMLFormElement;
const formData = new FormData(form);

const email = formData.get("email");
const username = formData.get("username");
const password = formData.get("password");
const color = formData.get("color");
const bio = formData.get("bio");
```

- reset
```
form.reset();
```
# 5. `Formik` Form
```
npm install formik --save
```
- `useFormik` hook
```
const formik = useFormik({ initialValues, submitHandler })
```

- create `submitHanlder`
- create validate
```
type ValidationErrorType = {
  email?: string;
  username?: string;
  password?: string;
  color?: string;
  bio?: string;
};
```

- add name, value, and `onChange` attribute
```
<input
className="form__input"
type="email"
id="email"
name="email"
value={values.email}
onChange={handleChange}
/>
<small className="error">{errors.email}</small>
```

- `destructuring formik` 
```
const { handleSubmit, values, handleChange, errors, resetForm } = formik;
```

# 6.  `Formik` Form With Components

```
import { Formik, Form, Field, ErrorMessage } from "formik";
```

- add name attribute to each input
- for select add `as="select"`
- for `textarea` add `as="textarea"`

- show errors
```
<ErrorMessage name="bio" className="error" component="small" />
```

- pass reset to `handleSubmit` as `{ resetForm }: { resetForm: () => void`
# 7. `Formik` Form With Components and Yup Validation

```
import * as Yup from "yup";
```

- set up `formSchema`
- use `validationSchema` option for Formik
- show errors
- pass reset to `handleSubmit`
```
validationSchema={formSchema}
validateOnChange={false}
validateOnBlur={false}
```

# 8. Query Parameters Form

- install `react-router-dom`
- surrounded route with router
- `useSearchParams` hook
- add name attribute to all inputs except password
- `handleChange` for inputs
- `document.getElementById` for password
- show a reset button

# 9. `LocalStorage / SessionStorage` Form

- create `getCurrentSotrage`
- give name attribute to all but password
- create `handleChange` function
- create `getInitialValues`
- submit handler
- show a reset button

----

# 10. `Next.js` / Tailwind Initial Styling

# 11. Form Data / Server Actions Form

# 12. `Search Params` Form

# 13. react-hook-form Form

# 14. react-hook-form form With `Zod` Validation

---

# Additional Videos Go Here