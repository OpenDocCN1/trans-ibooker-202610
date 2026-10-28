```
```python
return power

def __repr__(self):
    """Returns the human-readable representation of a polynomial.

    Notes
    -----
    For simplicity, we are returning the result of `__str__`. We
    should be returning something like
    UniPoly(2, 'x', 6) + UniPoly(1, 'x', 3) + UniPoly(-1, 'x', 0)

    Returns
    -------
    str
    """
    return str(self)

def __str__(self):
    """Creates a human-readable string representation.

    This returns forms that look like 2x**6 + x**3 - 1.

    Returns
    -------
    str
        Mathematical human-readable form of the polynomial.
    """
    if not self.__terms:
        return '0'

    def format_term(coefficient, exponent):
        """Format a single term in the polynomial.

        This function formats a term and handles the special
        cases when the coefficient is +/- 1 or the exponent is
        0 or 1.

        Parameters
        ----------
        coefficient : int
            The coefficient of the term.
        exponent : int
```
```