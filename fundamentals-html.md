1. What is the use of link tag inside the html file? <link rel="stylesheet" href="/index.css">
2. What is the use of script tag inside the html file? <script src="/index.jsx" type="module"></script>
3. why do we write type='module' in script tag?
4. eg of inline elements - <a>, <span> <img> they are appear on the same line
5. block level elements - appear on the different line <p> <h1> <div>
6. what is difference between input type='submit' and button element while using in react forms
7. what is difference between event.target and event.currentTarget in forms
8. what is new formData(event.currentTarget)
9. label htmlFor="email"
10. input id="email" defaultValue="joe@schmoe.com" type="email" name="email" placeholder="joe@schmoe.com" />
11. <textarea id="description" name="description" defaultValue="This is a description"></textarea>
12. fieldset, legend,
13. input type="radio" name="employmentStatus"
14. form action={signUp}
15. input type="radio" name="employmentStatus" value="part-time" , usage of value
16. <input type="radio" name="employmentStatus" defaultChecked={true} value="full-time" />
17. input type="checkbox" name="dietaryRestrictions" defaultChecked={true} value="full-time"
18. function signUp(formData) { // no need to pass event, no need to use event.preventDefault
    const email = formData.get("email") // input
    const employmentStatus = formData.get("employmentStatus") // radio
    const dietaryRestrictions = formData.getAll("dietaryRestrictions") // checkbox
    console.log(dietaryRestrictions)
  }

19. <label>
            <input type="checkbox" name="dietaryRestrictions" defaultChecked={true} value="gluten-free" />
            Gluten-free
        </label>

20.<select id="favColor" name="favColor" defaultValue="" required>
          <option value="" disabled>-- Choose a color --</option>
          <option value="red">Red</option>
21. 


